SQL Portfolio
--murder that occurred sometime on Jan.15, 2018
--and that it took place in SQL City
SELECT \*
FROM crime_scene_report
WHERE type = 'murder'
AND city = 'SQL City'
AND date = 20180115

--2 witnesses
-- 1st - lives at the last house on "Northwestern Dr" = 14887 Morty Schapiro
-- 2nd - Annabel, lives somewhere on "Franklin Ave" = 16371 Annabel Miller
Select \*
FROM person
WHERE address_street_name = "Franklin Ave"
AND LOWER(name) LIKE '%Annabel%'
LIMIT 10

--find their interview transcripts
Select \*
FROM interview
WHERE person_id IN (14887,16371)

-- The membership number on the "Get Fit Now Gym" bag started with "48Z". Only gold members
-- The man got into a car with a plate that included "H42W"
-- January the 9th
Select p._, gfci._
FROM drivers_license as dl
INNER JOIN person as p on dl.id = p.license_id
INNER JOIN get_fit_now_member as gfm on p.id = gfm.person_id
INNER JOIN get_fit_now_check_in as gfci on gfm.id = gfci.membership_id
WHERE plate_number LIKE "%H42W%"
AND membership_status = 'gold'
AND check_in_date = 20180109

-- query the interview transcript of the murderer to find the real villain behind this crime
Select \*
FROM interview
WHERE person_id = 67318

-- hired by a woman
-- 5'5" (65") or 5'7" (67")
-- red hair and she drives a Tesla Model S
-- attended the SQL Symphony Concert 3 times in December 2017.
WITH CTE AS (
SELECT
person*id,
event_name,
COUNT(*)
FROM facebook*event_checkin
WHERE date >= 20171201
AND date <= 20171231
GROUP BY person_id,
event_name
HAVING COUNT(*) >=3
)

SELECT p._, FB._
FROM drivers_license as dl
INNER JOIN person as p on dl.id = p.license_id
INNER JOIN CTE as fb on fb.person_id = p.id
WHERE hair_color = 'red'
AND gender = 'female'
AND height >= 65
AND height <= 67
AND car_make = 'Tesla'
AND car_model = 'Model S'
