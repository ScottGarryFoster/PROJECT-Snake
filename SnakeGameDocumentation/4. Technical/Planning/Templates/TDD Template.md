
*Tags go here*

|Owner|State|Last_update|
|--|--|--|
|USERNAME|Draft, Development, Review, Signed off|Date|

**Table of contents**
- [Purpose](#Purpose)
- [Key Requirements](#Key_Requirements)
- [Front Facing Features](#Front_Facing_Features)
- [Technical Design](#Technical_Design)
- [Data Pipeline](#Data_Pipeline)
- [Risks](#Risks)
- [Tasks](#Tasks)
- [Test Plan](#Test_Plan)
- [Feedback Plan](#Feedback_Plan)


# Purpose
>*In one sentence describe the purpose of this project.*

>*Now describe the purpose in more words without going into specifics of values or numbers, just the overall purpose and reason we are doing this work.*

---
# Key_Requirements
>*A list of requirements to hit for this to be complete.*

---
# Front_Facing_Features
>*How the feature (if any) is to interact or be interacted with from the outside world. As a plugin or command this is the UX/UI of that. As purely technical how something would technically plugin to this.*

---
# Technical_Design
>*Go into the literal technical design. Use UML where it helps. You do not need to go too in depth, for instance the exact methods you write - this is design not implementation. Therefore, just public facing design is required.*

```plantuml
class name
```

---
# Data_Pipeline
>How is data passed in and out of this? Is there any significant data passed around which might be able to be used by external plugins? This is also a good point for configuration files.*

## Input Data

|Input|Trigger|Type|Reason|
|---|---|---|---|
|Input Name|What happens to trigger input|Data type and location from|Why are we talking this input?|

## Actionable Data

|Input|Trigger|Reaction|Reason|
|---|---|---|---|
|Input Name|What triggered this action?|What happened to the data?|Why did we do this?|

## Output Data

|Input|Trigger|Type|Reason|
|---|---|---|---|
|Output Data Name|What triggered the output?|Where is it going?|Why are we outputting it here?|

---
# Risks
>*What are the risks to this project. How might this fail, how might the estimations be too short.*

|Risk|Level (H/M/L)|Reason for Level|Mitigation|
|---|---|---|---|
|User/Functional Requirements changing during development|||
|Application and system architecture|||
|Performance|||
|Human Resources|||

---
# Tasks
>List the tasks by name, in order, with descriptions and with estimations. Tasks should be releasable. So do not write “Data can be saved” “Data can be loaded” instead “Persistent data between restarts”. Each should build up to a testable user story however you may break down the tasks further to create better estimations.*

|Name|Description|Estimation (hrs)|
|---|---|---|
|Summary Line Name|What you expect to be complete|Esitmate the work in hours or T-Shirt. The idea is comparison rather than promise|

---
# Test_Plan
>What is the test plan. In terms of unit/integration testing what might be a good line of classes you’d like to integration test? In terms of top down, what QA level testing would you like to see.*

---
# Feedback_Plan
>Given these are public facing what is the feedback plan, do you seek this via a channel in Discord? Are we doing this for a week, month? Is this a private beta as we go with customers?*