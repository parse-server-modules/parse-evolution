# Feature name

* Proposal: 0001-brute-force-attack-detection
* Authors: [Simon Berry](https://github.com/simonaberry)
* Review Manager: TBD
* Status: **Awaiting review**

## Introduction
## Motivation

Cyberthreats are a real problem. Any servers running Parse Server are susceptible to hack attacks. The simplist would be a brute force attack - as there are no in built limits to login attempts. This is particulaly relevant as the default ACL on any new user added is Public Read - so it is fairly straight forward to get the usernames for all the users on a vanilla Parse Server (unless the developer has been good about changing teh User class ACLs). 

## Proposed solution

* Introduce a paramater that allows the developer to specify the maximum number of incorrect attempts (configurable) on a specific username before 'freezing' the account for a given time frame  (configurable)

OR

* Introduce an 'AfterLogin' hook in cloud code that resolves a promise if login was successful or rejects a promise if login was unsuccessful. This would allow the user to write his own logic to implement an account freeze if a certain number of incorrect logins were attempted

## Detailed design

Don't know the Parse Server code well enough to suggest detailed implementation

## Alternatives considered

could also possibly monitor the log files using a library like this https://github.com/rfxn/brute-force-detection

