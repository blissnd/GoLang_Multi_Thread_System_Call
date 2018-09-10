# BUG_IN_GOLANG
Demonstration of a critical concurrency bug in GoLang to try to persuade Google to fix it...
This is demo of what I consider to be a critical bug in the GoLang runtime that I would like Google to fix. 

It is a demo of concurrent code that should be re-entrant but does not appear to be. The source code and results from running it
demonstrate and provide evidence of the exact issue.

> go build test_concurrency.go
> ./test_concurrency.go

Running the functions sequentially:

Current directory from change_into_directory_1(): /home/blissnd/Documents/BUG_IN_GOLANG/DIR_1
*** This is file 1 ***

Current directory from change_into_directory_2(): /home/blissnd/Documents/BUG_IN_GOLANG/DIR_2
*** This is file 2 ***


--------------- Running the functions in parallel ---------------

Current directory from change_into_directory_2(): /home/blissnd/Documents/BUG_IN_GOLANG/DIR_2
Current directory from change_into_directory_1(): /home/blissnd/Documents/BUG_IN_GOLANG/DIR_2
*** This is file 2 ***

<<< ERROR >>>

--------------------------------------------------------------------------------------------------------
As shown, the O/S system calls to change the directory in thread_1 & thread_2 [functions change_into_directory_1() & change_into_directory_2()]
are not remaining indenpendent of one another. They seem to be sharing system state resources underneath, implying the code is 
not re-entrant.

EXPECTED BEHAVIOUR: That thread_2 does not reflect the current O/S directory state of thread_1 and operates independently of thread_1
including at the O/S level, so that the functions which are run in parallel remain fully re-entrant from the point-of-view of
the caller.