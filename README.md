# supply-chain-anomaly-
// Spark Streaming Job in Scala for Real-Time Anomaly Detection
// Assumes data from Kafka topics: shipment_updates, inventory_levels
// Output anomalies to Kafka topic: anomaly_events

import org.apache.spark.SparkConf
import org.apache.spark.streaming.{Seconds, StreamingContext}
import org.apache.spark.streaming.kafka010._
import org.apache.kafka.common.serialization.StringDeserializer
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types._
import org.apache.spark.streaming.dstream.InputDStream

object SupplyChainAnomalyDetector {
  def main(args: Array[String]): Unit = {
    val conf = new SparkConf().setAppName("SupplyChainAnomalyDetector").setMaster("local[*]")
    val ssc = new StreamingContext(conf, Seconds(10))

    val kafkaParams = Map(
      "bootstrap.servers" -> "localhost:9092",
      "key.deserializer" -> classOf[StringDeserializer],
      "value.deserializer" -> classOf[StringDeserializer],
      "group.id" -> "supply-chain-group",
      "auto.offset.reset" -> "latest",
      "enable.auto.commit" -> (false: java.lang.Boolean)
    )

    val topics = Array("shipment_updates")
    val stream: InputDStream[ConsumerRecord[String, String]] =
      KafkaUtils.createDirectStream[String, String](
        ssc,
        LocationStrategies.PreferConsistent,
        ConsumerStrategies.Subscribe[String, String](topics, kafkaParams)
      )

    val spark = SparkSession.builder.config(conf).getOrCreate()
    import spark.implicits._

    stream.foreachRDD { rdd =>
      if (!rdd.isEmpty()) {
        val rawDF = spark.read.json(rdd.map(_.value()))

        // Example schema: truck_id, timestamp, lat, lon, delay_min
        val cleanedDF = rawDF.filter("delay_min IS NOT NULL")

        // Define anomaly: delay over 30 mins
        val anomaliesDF = cleanedDF.filter("delay_min > 30")

        anomaliesDF.withColumn("anomaly_type", lit("DELAY"))
          .select("truck_id", "timestamp", "delay_min", "anomaly_type")
          .write
          .format("kafka")
          .option("kafka.bootstrap.servers", "localhost:9092")
          .option("topic", "anomaly_events")
          .save()

        anomaliesDF.show() // for debugging
      }
    }

    ssc.start()
    ssc.awaitTermination()
  }
}
