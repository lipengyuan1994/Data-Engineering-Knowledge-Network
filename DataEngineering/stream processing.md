* Data that is generated continuously
* Unbounded
* Often ordered by time
* Perform operations on subset of data in the stream
* Window describes what data is included


### Operations on Windows
* Aggrations
	* sum
	* average
* Sliding Windows


### Considerations 
* At least once vs guaranteed once processing
* late arriving data; How long do you want to wait before calculating. 
	* Ignore it , discard it
	* exception handling process. deal with late data separately.  but it has more complex logic
	* Buffer sizing; 