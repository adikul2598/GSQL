# GSQL

# SELECT CLAUSE

start = {Input User};
Friends = SELECT t FROM Start :s-(IsFriend:e) - user:t;
PRINT Friends;

start = {Input User};
Friends = SELECT t FROM Start :s-(IsFriend:e) - user:t;
WHERE e.connectDT BETWEEN to_datetime('2018-01-01') AND to_datetime("2019-01-01")
AND t.gender == "F"

QUERY FUNCTION 

CREATE QUERY GetFriends(vertex:User = inputuser) FOR GRAPH SOCIAL {


# ACCUM CLAUSE

FROM: select active vertices & edges.
#WHERE: Conditionally filfer the active sets
ACCUM: iterate on edge set; compute with accumulators
POST-ACCUM: iterate on vertex sets; compute with accumulators
HAVING: Conditionally filter the result set
ORDER BY: sort
LIMIT: max number of items
SELECT: result from sOurce or target set



