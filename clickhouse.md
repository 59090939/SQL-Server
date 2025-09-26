⬤ 'ShardedAndReplicated
CREATE TABLE local_table ON CLUSTER default (
    Year UInt16,
    Quarter UInt8,
    Month UInt8,
    DayofMonth UInt8,
    DayOfWeek UInt8,
    FlightDate Date,
    FlightNum String,
    Div5WheelsOff String,
    Div5TailNum String
)ENGINE = ReplicatedMergeTree
 PARTITION BY toYYYYMM(FlightDate)
 PRIMARY KEY (intHash32(FlightDate))
 ORDER BY (intHash32(FlightDate),FlightNum)
 SAMPLE BY intHash32(FlightDate)
SETTINGS index_granularity= 8192;

CREATE TABLE distributed_table ON CLUSTER default
AS xjp_ck_shared_db.local_table 
ENGINE = Distributed(default, xjp_ck_shared_db, local_table, rand());

insert into distributed_table select toDate('2024-03-25'),number,number+10000,number % 128,number,now(),'x','j','p' from numbers(10000)


⬤ Non-Replicated vs. Replicated Table Engines
MergeTree                      < ---------- > ReplicatedMergeTree                                                                                                     
SummingMergeTree               < ---------- > ReplicatedSummingMergeTree                  
ReplacingMergeTree             < ---------- > ReplicatedReplacingMergeTree                              
AggregatingMergeTree           < ---------- > ReplicatedAggregatingMergeTree                                        
CollapsingMergeTree            < ---------- > ReplicatedCollapsingMergeTree                             
GraphiteMergeTree              < ---------- > ReplicatedGraphiteMergeTree
VersionedCollapsingMergeTree   < ---------- > ReplicatedVersionedCollapsingMergeTree 
