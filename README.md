# Global-Functions-Documentation

_______________________________________________________________

sUNC is a tester for a executors capability to use functions under the Unified Naming Convention, people fail to realize how there is no difference in how certain functions should operate if a executor is external or internal, there is absolutely no difference and sUNC tests functions based on full functionality & not if they are half-assed or anything.

Functions like getcustomasset are barely talked about much, almost forgotten, but over 90% of executors fail to understand its functionality & usually spoof if via a string & dont care to implement it.

the only executors that ive seen that has the correct implementation of getcustomasset is Synapse Z, Wave, & Solara ( i could be wrong there could be more out there that actually have a working one )

Alot of executor owners get mad at sUNC due to their executors functions failing to pass sUNC tests, most of the time these are problems with the executor & not sUNC, just because your half-ass getgc does not pass on sUNC dosent mean sUNC is faulty.

Yes UNC is deprecated, Yes the docs are very vague to some point, but this does not make an exception on why you should be returning GetService on getnamecallmethod, or returning ab on a debug function just to spoof it.

sUNC is still being developed, some tests can have issues & are actively being fixed & on-top of that new tests are being implemented every single day.

Most Executors tend to spoof their functions of course to fake results on the regular UNC test, and refuse to show any results on sUNC because they label it as Faulty or Non-Functional or Messed Up & Want to promote their external xeno paste as the best.

Prime Examples:
- Blitz: Past Developer of ignite, repeatedly talks about how sUNC isnt made to test for externals
| Answer: It does not matter if you are external or internal, if you fail you fail
- RoHook: they advertised with a UNC of 86%, when sUNC was ran they were at 63% & they made numerous excuses on why they were at 63%
- Krucus: advertised at 100% UNC, was exposed for being around 60% with sUNC in which they too called sUNC faulty
- dna: has claimed sUNC to not be able to test external executors functions correctly
| Answer: Same as I said for blitz, It does not matter if you are external or internal, if you fail you fail

In my prime examples I called out people & executors for their opinions in which have been debunked or I debunked above, they are wrong & do not understand there are no differences between how a function should work on an executor if its external or not.

that is like saying getgenv().meow on an internal does not work the same on an external, make it make sense.

sUNC utilizes multiple tests for each function, as I mentioned before sUNC tests based on full functionality, you should not be whining about if some of YOUR functions are failing on sUNC, this is either due to you spoofing or your function does not fully function correctly. Some tests on sUNC rely on other functions, functions rely on other functions to be able to be tested, sUNC usually runs more tests depending on if you have those functions.

FileSystem relies on almost every function in the filesystem category to actually test eachother.
Loadstring depends on getscriptbytecode for the [basic] loadstring test on sUNC.

Most executors usually code their getinstances by doing game:GetDescendants(), if you really understood how UNC functions work you would understand this is not how this function works at all, it must access outside of game aswell.

I have also seen executors not have a fully implemented getrenv which is surprising, most of them do not contain the full environment of the games client.
Seliware did not have debug.info in their getrenv, it is fixed now though
MantiWPF does not have debug.info in their getrenv
Artemis & Athena do not have debug.info in their getrenv

Note: add error handling in your custom functions & actual checks for your renderproperty functions

It does not matter if your level 3 external paste cannot fully make this function work, it will Fail because your executor lacks the capabilities to fully implement said function.

Some executors code getrunningscripts in luau by just checking the entirety of the game & returning every script, also a horrible implementation & not how the function works at all, this is how getscripts is suppose to work.
getrunningscripts should return every script that actually has its thread actively running, in Synapse V3 you could check scripts & if they were actually running.

Most executors fake Websocket on UNC by just adding filler functions & itll pass regardless, which is why sUNC should be preferred over UNC due to UNCs incapability to thoroughly test functions.

Most level 3 executors setthreadidentity just add onto some number & actually dont set the threads identity, UNC fails to fully test this as it just checks if it returns an int
same goes for getthreadidentity, usually executors just return the number of the executors identity, getthreadidentity should return the identity of the current thread it is being ran on

Most executors utilize coroutines to make things like newcclosure in lua, this is also not a actual correctly written function inside your executor, self-explanatory, its in luau, more like its a bad implementation as listed by sUNC.

there are ways to make some functions like setreadonly in luau, most executors do this by rewriting the entirety of the table library in order to setup a custom system to unfreeze tables, some even make an entirely new environment & a spoofed datamodel to recreate metamethods to implement a form of hookmetamethod.

Executor owners get angry at sUNC due to them believing their half-ass functions fully work, though It is pretty common now to see that nobody truly understands how most of the functions listed in the Unified Naming Convention actually work.

_______________________________________________________________

Conclusion:
- Do as best as you can to not make your functions in luau
- Do not fake your functions, i dont know how this benefits you unless you are ratting or exit scamming
- If you cant fully implement a function, do not add it
- Normalize sUNC, you should understand UNC does not thoroughly test functions & it is beyond spoofable
