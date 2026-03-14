
🚀 Power BI Tip: Calculate % of Tickets Closed Within N Days — Without Creating Multiple Measures
Problem Statement
In a call center reporting scenario, stakeholders often want to track the percentage of tickets closed within specific time thresholds, such as:
•	Within 1 day
•	Within 2 days
•	Within 3 days
•	…and so on.
A common approach is to create separate measures for each threshold. However, this leads to:
❌ Too many measures
❌ Model clutter
❌ Higher maintenance effort
________________________________________
💡 The Smarter Approach
Instead of creating multiple measures, I built a dynamic DAX solution using an unrelated table that calculates the percentage of tickets closed within any selected number of days.
This makes the model:
✅ Cleaner
✅ Easier to maintain
✅ Fully dynamic for reporting
Swipe through to see how it works 👇
📊 Base Measure
Ticket Count = COUNTROWS('Ticket Data')
________________________________________
📈 Dynamic Measure: % Closed Within Selected Days
Percentage Closed within Days = 
VAR Day = SELECTEDVALUE('Closing Days'[Days])

VAR TicketCntWithinDays =
    IF(
        Day <= 10,
        CALCULATE(
            [Ticket Count],
            'Ticket Data'[Days Taken to close] <= Day
        ),
        CALCULATE(
            [Ticket Count],
            'Ticket Data'[Days Taken to close] > 10
        )
    )

RETURN
DIVIDE(TicketCntWithinDays, [Ticket Count])

📊 Conditional formatting Measure
CF 70 percent = IF([Percentage Closed within Days]>=0.7,"#b6fc8b",BLANK())



