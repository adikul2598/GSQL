# SELECT CLAUSE

1. start = {Input User};  
Friends = SELECT t FROM Start :s-(IsFriend:e) - user:t;  
PRINT Friends;  

2. start = {Input User};  
Friends = SELECT t FROM Start :s-(IsFriend:e) - user:t;  
WHERE e.connectDT BETWEEN to_datetime('2018-01-01') AND to_datetime("2019-01-01")  
AND t.gender == "F"  

## QUERY FUNCTION  

CREATE QUERY GetFriends(vertex:User = inputuser) FOR GRAPH SOCIAL {  


# ACCUM CLAUSE  

FROM: select active vertices & edges.     
WHERE: Conditionally filfer the active sets  
ACCUM: iterate on edge set; compute with accumulators  
POST-ACCUM: iterate on vertex sets; compute with accumulators  
HAVING: Conditionally filter the result set  
ORDER BY: sort  
LIMIT: max number of items  
SELECT: result from sOurce or target set  

## Map Accum

What is age distribution of friends that were registered in 2018  

CREATE QUERY GetFriends (vertex<User> inputUser) FOR GRAPH Social  
MapAccumc<uint, SumAccum<uint>> @@ageMap;  
Start = (inputUser);  
Friends = SELECT t FROM Start:s-(IsFriend:e) - :t  
wHERE e.connectDt BETWEEN to_datetime ("2018-01-01")  
AND to_datetime ("2019-01-01")  
ACCUM @@ageMap += (t.age/10->1);  
PRINT @@ageMap  
  
## AVG Accum
Given an input user. Output the average age of their common friends.  
  
CREATE QUERY GetFriends (vertex<User> inputUser) FOR GRAPH Social (  
AvgAccum @@avgAge ;  
Start = {inputUser };  
FriendalHop  =  SELECT  t FROM Start:s- (IsFriend:e)-:t;  
Friends2Hop = SELECT t  
FROM Friends1Hop:s- (IsFriend: e) -: t  
ACCUM t.@avgAge +=  s.age:  
print Friends2Hop;  
  
## POST ACCUM
  
Given a set of persons (friends of friends of theinputUser), output the normalized number of common friends for each person in the set.  
  
CREATE QUERY GetFriends (vertex<User> inputUser) FOR GRAPH Social {  
SumAccum<uint> @@Num;  
SumAccum<float> @@normCNum;  
MaxAccum<float> @@maxCNum ;  
Start {inputUser} ;  
Friends1Hop =  SELECT t FROM Start : s- (IsFriend:e) - :t ;  
Friends2Hop = SELECT t  
                            FROM Friends1Hop:s- (IsFriend:e) - :t  
                            ACCUM t.@cNum += 1  
                            POST-ACCUM @@maxCNum += t.@cNum;  
Friends2Hop = select s FROM Friends2Hop: s  
POST-ACCUM  
s.@normCNum =  s.@cNum/@@maxCNum;  
print Friends2Hop;  
  
# HAVING, ORDER BY AND LIMIT CLAUSE

  Given a set of selected vertices (friends of friends of the inputUser). output those whose friends in common with the inputUser have an average age over 30.

CREATE QUERY GetFriends (vertex<User> inputUser) FOR GRAPH Social {   
AvgAccum @avgAge ;  
Start = {inputUser};  
Friends1Hop  = SELECT t FROM Start:s-(IsFriend: e)- :t ;  
Friends2Hop  = SELECT t  
FROM Friends1Hop: s-(IsFriend : e) -:t  
ACCUM t.@avgAge += s.age  
HAVING t.@avgAge > 30;  
print Friends 2Hop;  



