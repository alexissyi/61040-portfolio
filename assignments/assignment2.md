## Problem Statement

### Problem Domain

**WILG Meal Plan** The WILG meal plan provides 6 dinners a week, all cooked by house members, and snacks and breakfast items. Executing this meal plan involves 1) creating a cooking calendar each month based on member availability, assigning members as cooks for each cooking day; 2) collating dinner ingredients on a weekly basis; 3) collating snack and breakfast item requests on a weekly basis; 4) ordering all the groceries through various channels, including produce vendors, meat vendors and Instacart; 5) tracking food allergies and dietary restrictions. These tasks are managed by two members of WILG Exec, known as foodstuds. Currently dinners must be enough to feed 35 people, with various dietary restrictions accommodated. I am president of WILG and also regularly cook for the house, so I'm interested in making this process easier for cooks and WILG foodstuds in particular.

### Problem

**[WILG Meal Plan Cooking Assignments]** Everything outlined above is done manually and across various spreadsheets, forms and even random Slack threads (WILG communication occurs via Slack). In particular, member cooking availabilities and preferences are collected via a form and then one of the foodstuds manually assigns cooks based on the calendar. This often requires multiple revisions since people's availabilities can change.

### Stakeholder List

1. WILG Food Stewards: the two members of WILG Exec who are in charge of creating a cooking calendar, assigning cooks, and ordering groceries. Any automation of their task could make their task easier; a more customized UI (in comparison to Google Sheets) could also make their task easier and reduce the amount of time they spend on their role obligations. Learning a UI or software could also be a barrier though.

2. WILG Cooks: the residents of WILG who are assigned to cook on WILG's cooking calendar. A more customized UI could make it easier to enter availability and update that availability, as well as communicate with the food stewards. Learning a new UI could also be a barrier like above.

3. WILG Residents: individuals who are automatically on WILG's meal plan and eat dinners from the WILG calendar. Automation of the cooking schedule can reduce the chance that there's no one assigned to one day or that someone has to drop and so we have to eat leftovers.

### Evidence and Comparables

1. [Paprika](https://www.paprikaapp.com/). A meal-planning app that allows for ingredient collating, linking of recipes, and scheduling meals. Good for an individual, but not super compatible with WILG's cooking system, which involves specific distinctions between ingredients and also involves assigning different people to different days.

2. [Meal planning is associated with food variety, diet quality and body weight status in a large sample of French adults](https://pmc.ncbi.nlm.nih.gov/articles/PMC5288891/). This article discusses how meal planning, at least on an individual basis, improves health metrics for adults.

3. [Kooka Cooking Community](https://www.kookacommunity.com/). A meal-planning/recipe-planning website that allows for scheduling and ingredient collection. Has similar limitations to Paprika.

4. [Why Communal Living Can Make Us Happier](https://www.bbc.com/culture/article/20240429-why-living-with-strangers-can-make-us-happier). This article discusses how communal living systems that involve chore schedules and cooking plans or other modes of shared responsibility can help provide community and alleviate burdens.

5. My personal experience as a member of WILG and current president. As president, I often have to help the foodstuds trouble shoot problems, like rearranging the cooking calendar or getting missing ingredients. Assigning cooks is one of the first tasks in the pipeline of executing the WILG meal plan and all the other aspects of dinner follow after this. As a cook, sometimes I have to let the foodstuds that my availability wasn't accommodated, so the assignments need to be updated.

## Application Pitch

### Name

CookScheduler

### Motivation

CookScheduler automates the assignment of cooks to different days in a communal cooking calendar in accordance with cooks' self-reported availability and cooking preferences.

### Key Features

#### Preference Form

Cooks will be able to upload their cooking preferences (distinct from availability) in a form available on the site. This form will allow them to specify whether they are willing to cook on their own and/or cook with another person; if they are willing to be main cook if they cook with someone else; if there's anyone they want to cook with; how many times they want to cook in the month. The form will lock after a certain deadline, at which point it can't be edited.

#### Availability Form

This form will have a calendar UI with clickable squares that allows cooks to mark themselves as available to cook on that day. The form will default to no availability on any day. The form will lock after a cetain deadline, at which point it can't be edited.

#### Editable Cooking Calendar

This calendar will display cooking assignments in a calendar UI. This UI will also have clickable date squares that allows the foodstud to see who is available and their preferences on each day. After the form deadline, cooking assignments will be automatically generated and displayed on this calendar, but the foodstuds will be able to edit the schedule and change the assignments if they want. Cooks will be able to view the calendar but not edit it.

## Concept Design

### Concept Specifications

1. **concept** TimeSensitiveForm\[User\]

   **purpose** take in user input for a set of questions

   **principle** after the form deadline, the responses to the form can be used to guide decisionmaking

   **state**

   a set of Questions with

   &nbsp; a string QuestionText

   &nbsp; a set of Responses

   a set of Responses with

   &nbsp; a User

   &nbsp; a string ResponseText

   a time Deadline

   **actions**

   submitResponse(user: User, question: Question, responseText: String)

   addQuestion(question: Question)

   deleteQuestion(question: Question)

   deleteResponse(user: User, question: Question)

   **system** lockForm()

2. **concept** PreferredRoles\[User\]

   **purpose** collect cooking role preferences for scheduling

   **principle** after collecting user preferences, cooking assignments can be made that only assign people one of their preferred roles each day they cook

   **state**

   a set of Users with

   &nbsp; a boolean CanSolo

   &nbsp; a boolean CanLead

   &nbsp; a boolean CanAssist

   **actions**

   upload(user: User, canSolo: boolean, canLead: boolean, canAssist: boolean)

   updateSolo(user: User, canSolo: boolean)

   updateLead(user: User, canLead: boolean)

   updateAssist(user: User, canAssist: boolean)

3. **concept** UserAvailabilities\[User, Month, Date\]

   **purpose** collect User cooking availability for scheduling

   **principle** after collecting user availability, cooking assignments can be created that only assigns cooks to days they are available

   **state**

   a Month

   a set of Users with
   a set of dates AvailableDates

   **actions**

   addAvailability(user: User, date: Date)

   removeAvailability(user: User, date: Date)

4. **concept** CookingAssignments\[UserAvailabilities, UserPreferences, User\]

   **purpose** track cooking assignments for the month so we know who cooks when

   **principle** after user availabilities and user preferences are uploaded, can automatically generate a set of cooking assignments, which can then be edited by foodstuds

   **state**

   a Month

   a set of Users

   a UserAvailabilities

   a PreferredRoles

   a set of dates CookingDates with
   a LeadCook
   an AssistantCook (optional)

   **actions**

   assignCook (user: User, date: Date, role: String)

   removeCook (user: User, date: Date)

   uploadPreferences (preferredRoles: PreferredRoles)

   uploadAvailabilities (userAvailabilities: UserAvailabilities)

   generateAssignments()

   validate()

### Synchronizations

## UI Sketches

## User Journey
