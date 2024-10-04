# Exploratory Analysis Journal

- [x] Establish assumptions
- [x] Calculate Basic metrics around reservations
- [ ] Agents performance exploration
    - [ ] Perform univariate data exploration
- [ ] Perform bivariate data exploration
- [ ] Perform multivariate data exploration
- [ ] Explore booking volume over time
- [ ] Explore booking by customer segment

---


## Assumptions

Before start our exploratory analysis, There's a set of questions I'd ask to stakeholders. Since approach of this project is open, let's summarize assumptions considered for the sake of challenge:

1. If a reservation is cancelled, there is not any fee associated to it.
2. avg_daily_rate numbers already include all taxes.
3. No extra cost for children or baby accomodations.
4. No extra cost for special request or required parking spaces.
5. We do pay comissions for reservations booked through an agent (25% over accomodation_rate, not final_rate) only for concrete reservations (ends up as Checkout)
6. For refundable reservations, Company must return whole final_rate for cancelled reservations
7. Disccounts are automatically embedded into accomodation_rate value (if applicable)
8. meal_cost is something guests need to pay along with their accomodation_rate. Meal cost for a reservation is calculatad considering 1 meal per day stay per adult and children. Babies don't pay meals. Children pay half of the meal rate.
9. final__rate is the total amount paid for a reservation. This number includes accomodation rates, meal costs and discounts if available for whole stay.
10. Challenge and rate calculations in model ignores any other type of discount apart from the one specified by MarketSegment table.
 

## Basic Metrics around reservations

For time intelligence convenience, I had to add a calulated column booking_date, which is the anticipated date user booked a room before their arrival. 

Added a calculated column called total_rate which represents total amount paid for a reservation
    `(stays_in_week_nights + stays_in_weekend_nights) * avg_daily_rate`


## Agents performance exploration

### Problem statement
    The cost of having an agent is getting too high, we are paying 25% commision rate to agents. We need to reduce agent cost by 15%, which agents would you recommend us to stop collaboration with?

 so I performed some calculations that could help to evaluate agent's performance:
1. **Produced Income**. Most important from my perspective, describes how much earnings represent for company. Only checked out reservations
2. **Booking Effectivenes**. A secondary parameter that shows ratio of checked out bookings / total bookings per agent. 
3. **Booking Volume**. A third less important parameter considered. It's less important in my opinion because more bookings does not necessarily imply better bookings (ones with high profits or that end up with checkouts) 

Metrics could be influenced by how long agents have worked for the company. It's not fair to evaluate an agent with 10 reservations achieved in 3 months than another agent with 10 reservations achieved in a year. Unfortunately, we don't have enough information to measure this fact. 


## Customer Loyalty exploration

### Problem statement
    We will be working with a marketing agency, we don't trust the agency yet to share with them our data. Based on your data analysis can you provide a definition of our loyal customers so the team can tweak their segmentations?

Reservations table has some useful variables we can use to formluate a definition of loyalty among our guests. 
The most relevant variables are `is_repeated_guest`, `previous_bookings_not_cancelled`, `previous_cancellations`.
To take advantage of such variables, we are going to calculate a couple factors that can help our analysis endevours:

TOTAL BOOKINGS = `previous_bookings_not_cancelled` + `previous_cancellations` + 1

**NOTE** (We add 1 because of the current reservation entry) 

CANCELATION COEFFICIENT = `previous_cancellations` + (1) / TOTAL BOOKINGS

**NOTE** (Add 1 as long as current reservation's `is_canceled` = 1)

## Extra Amenities exploration

### Problem statement
    We have an hypothesis that customers with babies are more likely to use our extra services (parking, meals etc). can you please validate if that is true?

For this hypothesis exploration, we are going to rely on 2 parameters available in our dataset:
- `total_special_requests` Extra services a customer may need during staying.
- `required_car_parking_spaces`. Spaces reserved for a customer

As a complementary study, we are going to get to know better our customers with babies by exploring following variables:
- `meal` To identify if there's an special preference of meal menu for customers with babies
- `market_segment` To Identify if customers with babies have a bigger density in certain segments.

I labeled reservations to classify 
with babies. If at least 1 baby is reported by customer
without babies. If customers don't bring babies to the hotel

As reservations without babies is vast greater (98.8%) than the ones without babies, then we are going to use rations to make a fair comparison. 

## Time Intelligence Exploration
    Client is very much interested in Time Analysis exploration (seasonality, festive periods, weekdays vs weekend) I want to know when I have low occupancy.
    Give me an overview of when I can offer deals (low demand) vs when I can increase prices (high demand)

Some useful fields for this analysis are:
`arrival_date` - To see effective days of occupancy

`active_reservations` - This measure can help to determine the number of active reservations given an specific date. 

Secondary fields to complement study:
`lead_time` - Time (in days) in advance a guest made a reservation before arrival
`is_canceled` - We can use this variable as a filter to classify effective bookings vs lost bookings

Given that 2019 is the only year we have full data, we are going to compare periodos of time vs same period previous year to perform a fair comparison.

## Other intriguing findings

While I was calculating total income for business, I played around with `is_cancelled`, deposity_type and reservation_status. I found 47 observations that seem to be incongruent, because they were cancelled , but also has status as Check-out. 

Some bookings end up with $0.00 rate. This is because some of them have 0 reported stay_nights, some others have 0 people (Some corporates), or simply avg_daily_rate is 0.

While I was exploring potential variables to define customer loyalty, I realized that `is_repeated_guest` sometimes has value of 1 (Assuming this means True), but either `previous_bookings_not_cancelled` and `previous_cancellations` have a value of 0. It's reasonable to expect previous booking history (some non cancellation bookings) on reservations marked as repeated guests; however this is not always the case in dataset. 