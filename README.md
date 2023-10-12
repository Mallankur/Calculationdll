# Calculationdll
dll
namespace CansolveAnkurLib
{
    public static class MongoDataCansolvrProcessor
    {

        public static double CalculateAverageForEventTime(IMongoCollection<BsonDocument> collection, long eventTime)
        {
            var aggregate = collection.Aggregate()
                .Match(Builders<BsonDocument>.Filter.Eq("EventTime", eventTime))
                .Group(new BsonDocument
                {
                    { "_id", BsonNull.Value },
                    { "average", new BsonDocument("$avg", "$Value") }
                });

            var result = aggregate.FirstOrDefault();
            if (result != null)
            {
                BsonDecimal128 avgDecimal = (BsonDecimal128)result["average"].AsDecimal;
                decimal avg = avgDecimal.ToDecimal();
                return (double)avg;
            }

            return 0.0;
        }

    }
}
