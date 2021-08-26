# SQL-code-for-Salary-Calculating-App
USE public;
SELECT 
ep.emp_id,
IFNULL(p.p_salary, 0) AS position_salary, 
IFNULL(r.r_salary,0) AS rank_salary, 
IFNULL(to_salary, 0) AS term_of_office_salary,
IFNULL(sl.salary, 0) AS skill_salary,
IFNULL(Food.amount, 0) AS food_salary,
IFNULL(extra.amount, 0) AS extra_20,
IFNULL(m_aid.amount, 0) AS material_aid,
IFNULL(q_x.amount, 0) AS Impeccable_service,
ROUND(IFNULL(p.p_salary, 0) + IFNULL(r.r_salary, 0) + IFNULL(to_salary, 0) +
		IFNULL(sl.salary, 0) + IFNULL(Food.amount, 0) + IFNULL(extra.amount, 0) +
        IFNULL(m_aid.amount, 0) + IFNULL(q_x.amount, 0) , 2) AS SUM_adds,
ROUND(((IFNULL(r.r_salary, 0) + IFNULL (p.p_salary, 0) - 200) *0.14), 2) AS income_tax,
ROUND((IFNULL(p.p_salary, 0) + IFNULL(r.r_salary, 0) + IFNULL(to_salary, 0) +
		IFNULL(sl.salary, 0) + IFNULL(Food.amount, 0))*0.03, 2) AS sosial_insurance,
IFNULL(Almny.amount, 0) AS alimony,
IFNULL(red_c.amount, 0) AS red_crescent,
IFNULL(t_union.amount, 0) AS trade_union,
IFNULL(delay_s.amount, 0) AS delay_salary,
ROUND(((IFNULL(r.r_salary, 0) + IFNULL (p.p_salary, 0) - 200) *0.14) +
	IFNULL(Almny.amount, 0) + IFNULL(red_c.amount, 0) + IFNULL(t_union.amount, 0) +
    IFNULL(delay_s.amount, 0) + (IFNULL(p.p_salary, 0) + IFNULL(r.r_salary, 0) + IFNULL(to_salary, 0) +
    IFNULL(sl.salary, 0) + IFNULL(Food.amount, 0))*0.03, 2) AS Sum_deduction,
ROUND(IFNULL(p.p_salary, 0) + IFNULL(r.r_salary, 0) + IFNULL(to_salary, 0) +
		IFNULL(sl.salary, 0) + IFNULL(Food.amount, 0) + IFNULL(extra.amount, 0) +
        IFNULL(m_aid.amount, 0) + IFNULL(q_x.amount, 0)  -
  ((IFNULL(r.r_salary, 0) + IFNULL (p.p_salary, 0) - 200) *0.14) +
	IFNULL(Almny.amount, 0) + IFNULL(red_c.amount, 0) + IFNULL(t_union.amount, 0) +
    IFNULL(delay_s.amount, 0) + (IFNULL(p.p_salary, 0) + IFNULL(r.r_salary, 0) + IFNULL(to_salary, 0) +
    IFNULL(sl.salary, 0) + IFNULL(Food.amount, 0))*0.03, 2) AS net_salary
FROM positions p
LEFT JOIN employee_positions ep 
		ON ep.position_id = p.id
LEFT JOIN employee_ranks er
		ON ep.emp_id = er.emp_id
LEFT JOIN ranks r
		ON er.rank_id = r.id
LEFT JOIN employee_skills es
		ON es.emp_id = ep.emp_id
LEFT JOIN skill_levels sl
		ON es.skill_id = sl.id
LEFT JOIN employee_adds_or_deductions eaod
		ON eaod.emp_id = ep.emp_id
LEFT JOIN add_or_deductions aod
		ON eaod.aod_id = aod.id
LEFT JOIN term_of_office_salary toos
		ON toos.emp_id = ep.emp_id
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 3 GROUP BY eaod.emp_id) Food ON (eaod.emp_id = food.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 7 GROUP BY eaod.emp_id) Almny ON (eaod.emp_id = almny.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 8 GROUP BY eaod.emp_id) extra ON (eaod.emp_id = extra.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 9 GROUP BY eaod.emp_id) m_aid ON (eaod.emp_id = m_aid.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 10 GROUP BY eaod.emp_id) q_x ON (eaod.emp_id = q_x.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 10 GROUP BY eaod.emp_id) red_c ON (eaod.emp_id = red_c.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 10 GROUP BY eaod.emp_id) t_union ON (eaod.emp_id = t_union.emp_id)
LEFT JOIN (SELECT IFNULL(amount, 0) AS amount, eaod.emp_id 
FROM employee_adds_or_deductions eaod WHERE eaod.aod_id = 10 GROUP BY eaod.emp_id) delay_s ON (eaod.emp_id = delay_s.emp_id)
GROUP BY ep.emp_id
