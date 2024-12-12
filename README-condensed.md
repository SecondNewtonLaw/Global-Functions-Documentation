# sUNC Global-Functions-Documentation

_______________________________________________________________

sUNC is a testing script for an executors' capability, utilizing the original [Unified Naming Convention](https://github.com/unified-naming-convention/NamingStandard). People fail to realize how there is no difference.

A lot of executor owners get mad at sUNC due to their executor's functions failing to pass certain checks. What the developers fail to realize, is that, for example, `getgc` doesn't mean a useless table, which is maybe sprinkled with some `game:GetDescendants()`. This is just one example out of many bad implementations which get flagged by the script. 

Now that the times have changed, UNC, for some reason, is now standardly used to check how good an executor is, which is not what it was made for. That's where sUNC comes in. 

sUNC has came a long way already. It is still being maintained. There were lots of bug fixes, new checks, and new tests added to the suggestion of the public, already widely existing or heavily wanted functions.

A lot of external executor owners misunderstand how the functions should behave. I have seen countless developers complain that their `cache` library is being failed. But when I ask them if they know what the roblox registry is, or how the cache library interacts with it, they freeze.

That's because they are used to the original UNC test being standardized by people as a deep testing script for functions.

It doesn't matter if your average level 3 external paste cannot fully make these functions work, they will fail because it lacks the capabilities to fully implement the said function.
Which is why sUNC should be preferred over UNC, due to thoroughly tested functions.

It is pretty common now to see that nobody truly understands how most of the functions listed in the Unified Naming Convention actually work, despite there being an "API" section which explains some more stuff.

_______________________________________________________________

# Conclusion

- Do as best as you can to not make your functions in luau.
- Do not fake your functions, this will ultimately not benefit your userbase.
- Normalize/spread sUNC. UNC does not thoroughly test functions & it is easily spoofable.

---

# Credits

- [Original UNC Documentation](https://github.com/unified-naming-convention/NamingStandard/tree/main)
