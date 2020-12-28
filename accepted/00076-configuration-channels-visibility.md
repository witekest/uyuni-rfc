- Feature Name: Enhance visibility of connected Configuration/State Channels
- Start Date: 2020-12-17
- RFC PR: 

# Summary
[summary]: #summary

Provide visibility and overview of **inherited** Configuration and State Channels connections at a System Details level. The reverse information as well should be provided by giving visibility of those systems they are connected (directly or by **inheritance**) to a certain Configuration or State Channel at a Channel Details level.

*Note: everything discussed in this RFC applies for Salt clients only. Traditional clients do not inherit Configuration channel assignments from System Groups/Organizations.*


# Motivation
[motivation]: #motivation

### Why are we doing this?

At the moment there is no way to figure out Configuration and State Channels connections other than the ones directly in place to a specific System. Such a type of channel can exist and be difined at:

- Organization level
- System Group level
- System level

but only the third level is visible for each System. This means that once Salt States are applied, there will be more than what is visible: Salt States are a set of defined states from

Organization Configuration Channels **+** System Group Configuration Channels **+** System Configuration Channels

### What use cases does it support?

Typical and complete use case:

1. create an Organization O1
2. create a System Group G1
3. onboard a System S1 as a Salt client in the Organization O1 and System Group G1
4. create a State Channel C1 at an Organization O1 level
5. create a State Channel C2 at a System Group G1 level
6. create a State Channel C3 directly at a System S1 detail level
7. go to System Detail > States > Configuration Channels > System
8. the C3 channel is listed as an assignment to S1
9. there is no info about C1 nor C2 channels, but because of step 4. 5. in reality S1 will receive not only C3 but also C1 and C2 as inheritance of its own setup, being part of O1 and G1
10. the reverse logic is valid as well, there is no information about systems assigned to channels C1 or C2 in Configuration > Channels > Channel Overview > Systems because. No system is directly assigned to them, but S1 inherits them.

Additional use case: a system can be part of multiple groups, therefore **inheritance** can become wide and complex really fast.


### What is the expected outcome?

The fact that with Salt *black magic* there is no way to figure out what will override others in the applied states, we cannot provide a reliable hierarchy.
What we can provide is a full set of assignments, no matter if directly assigned or inherited by other assignments.

# Detailed design
[design]: #detailed-design

TODO

{
  - data are already available in the database
  - identify the proper way to show them
  - how to interact, if any interaction is expected

}


# Drawbacks
[drawbacks]: #drawbacks

Why should we **not** do this?

- No reason for not doing this

# Alternatives
[alternatives]: #alternatives

- What other designs/options have been considered?
TODO

- What is the impact of not doing this?
Receiving bugs, unsatisfied users, inconsistency between *expectations* and *reality* in terms of applied states at a System level.

# Unresolved questions
[unresolved]: #unresolved-questions

- Performance issues only bounded to pages where the information is shown: displaying data can cost if there is a very huge set of Channels and Systems and assignments are spread around to all the levels in a mixed way. But it is unrealistic since typical expected very large usage of Configuration Channels is up to dozens of channels, sometimes up to 50, but below 100.
- **We cannot provide a reliable hierarchy** or a graphs of **inheritances** based on who overrides who due to the internal Salt logic on how to apply the whole set of states.
