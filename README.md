# QA-Test
In this repo is the QA Test Cases written for the Broken Hashserve app. 

My Process:

After reading the doc emailed to me, I took the acceptance criteria from within the doc and reformatted it in the test case format needed to run an end-to-end test. I noted in the test case areas that are uncertain to me and that would require confirmation from dev (auth token for pastword POST, entering gibberish in password field). After having created the plan, I pulled the file down and installed it on my machine, and began to execute the plan.

RESULTS: 

Bugs: 
1.3 Step 2: Percentage value returning instead of SHA512 hash

Instead of recieving a SHA512 hash as a return, I instead recieved an incrimental n% (the first return was 1%, the second 2%, the 3rd 3%, etc). I entereted the angrymonkey password on the first request, but gibberish on the subsequent. Neither made a difference on the returns.

Steps for Repro:

1.	Open a terminal window
2.	Enter “curl -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash”

Results: Observe that instead of the specified SHA512 hash, what is returned is a single incriminting digit.



1.5 Step 3: 200 message not displaying after successful shutdown

I was able to trigger the shutdown, and verified that an existing process was allowed to complete and a process after shutdown was not allowed to initiate. One note however, is that I did not recieve a 200 response in the terminal from the POST. The app server shut down successfully, though.

Steps for Repro

1.	Open a terminal window
2. Launch the server app
3.	Enter “curl -X POST -d ‘shutdown’ http://127.0.0.1:8088/hash”

Results: Observe that, although the app will shutdown gracefully, a 200 response is never printed in the terminal after execution



Questions:

1. I ran the POST curl for the password and as it was processing, also ran the stats curl command. The Stats came back as reading 4 total requests, but counting the request that was in process for the POST, this should have read 5. Do the stats counts get counted only upon completion of the request, or when the request is triggered?

2. Is an auth token not needed as part of the payload for the password POST request? Or is this intented to be unauthenticated?
