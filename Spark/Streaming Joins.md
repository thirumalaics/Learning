## Stream-static joins
- expressing joins in terms of code remains the same
	- only the reading of the data is diff
- inner, left outer(when the left side is a streaming dataframe), right outer(when the right side is a streaming dataframe)
	- other joins not supported because they are not easy to run incrementally
- stream-static joins are stateless, therefor do not require any kind of wming
- the static dataframe is read repeatedly while joining with the streaming data of every micro-batch, so we can cache static DF
- if the underlying data source on which the static dataframe was defined changes, whether those changes are seen by the streaming query depens on the specific behaviour of the data source
	- in case of files, new files are not detected until the streaming query is restarted
- good for enriching the stream data with slowly changing data

## Stream-Stream joins
- challenge: at any point in time, the view of either dataset is incomplete which makes finding matches hard
- the matching events b/w the two streams might arrive in any order and may be arbitrarily delayed
- SS accounts for such delays by buffering the i/p data from both sides as streaming state, and continuously checking for matches as new data is received
- the code used for join does not change here
- execution completely different
- events pertaining to events of dataframes from both sides of joins are buffered