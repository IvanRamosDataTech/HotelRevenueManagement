# Exploratory Analysis Journal

- [x] Establish assumptions
- [x] Calculate Basic metrics around reservations
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

## Other intriguing findings

While I was calculating total income for business, I played around with is_cancelled, deposity_type and reservation_status. I found 3 observations that seem to be incongruent, because they were cancelled , but also has status as Check-out. 

Some bookings end up with $0.00 rate. This is because some of them have 0 reported stay_nights, some others have 0 people (Some corporates), or simply avg_daily_rate is 0.