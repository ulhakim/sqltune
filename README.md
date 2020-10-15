hi to who will review my answer 

this is answer to assignment B. 

as what i see it is SQL tuning.

the original query is as follow and the commented line is my comment


##### start original query #####
```

/* TOO MUCH SELECT COLUMNS AND SOME OF THESE ARE SEEM NO PURPOSE , ADVISE TO MINIMIZE AND TAKE THE NEEDED ONLY*/
SELECT Jobs.id AS `Jobs__id`,
       Jobs.name AS `Jobs__name`,
       Jobs.media_id AS `Jobs__media_id`,
       Jobs.job_category_id AS `Jobs__job_category_id`,
       Jobs.job_type_id AS `Jobs__job_type_id`,
       Jobs.description AS `Jobs__description`,
       Jobs.detail AS `Jobs__detail`,
       Jobs.business_skill AS `Jobs__business_skill`,
       Jobs.knowledge AS `Jobs__knowledge`,
       Jobs.location AS `Jobs__location`,
       Jobs.activity AS `Jobs__activity`,
       Jobs.academic_degree_doctor AS `Jobs__academic_degree_doctor`,
       Jobs.academic_degree_master AS `Jobs__academic_degree_master`,
       Jobs.academic_degree_professional AS `Jobs__academic_degree_professional`,
       Jobs.academic_degree_bachelor AS `Jobs__academic_degree_bachelor`,
       Jobs.salary_statistic_group AS `Jobs__salary_statistic_group`,
       Jobs.salary_range_first_year AS `Jobs__salary_range_first_year`,
       Jobs.salary_range_average AS `Jobs__salary_range_average`,
       Jobs.salary_range_remarks AS `Jobs__salary_range_remarks`,
       Jobs.restriction AS `Jobs__restriction`,
       Jobs.estimated_total_workers AS `Jobs__estimated_total_workers`,
       Jobs.remarks AS `Jobs__remarks`,
       Jobs.url AS `Jobs__url`,
       Jobs.seo_description AS `Jobs__seo_description`,
       Jobs.seo_keywords AS `Jobs__seo_keywords`,
       Jobs.sort_order AS `Jobs__sort_order`,
       Jobs.publish_status AS `Jobs__publish_status`, /* MAYBE THIS COLUMN SHOULD NOT BE CALL */
       Jobs.version AS `Jobs__version`,
       Jobs.created_by AS `Jobs__created_by`,
       Jobs.created AS `Jobs__created`,
       Jobs.modified AS `Jobs__modified`,
       Jobs.deleted AS `Jobs__deleted`, /* MAYBE THIS COLUMN SHOULD NOT BE CALL */
       JobCategories.id AS `JobCategories__id`,
       JobCategories.name AS `JobCategories__name`,
       JobCategories.sort_order AS `JobCategories__sort_order`,
       JobCategories.created_by AS `JobCategories__created_by`,
       JobCategories.created AS `JobCategories__created`,
       JobCategories.modified AS `JobCategories__modified`,
       JobCategories.deleted AS `JobCategories__deleted`, /* MAYBE THIS COLUMN SHOULD NOT BE CALL */
       JobTypes.id AS `JobTypes__id`,
       JobTypes.name AS `JobTypes__name`,
       JobTypes.job_category_id AS `JobTypes__job_category_id`,
       JobTypes.sort_order AS `JobTypes__sort_order`,
       JobTypes.created_by AS `JobTypes__created_by`,
       JobTypes.created AS `JobTypes__created`,
       JobTypes.modified AS `JobTypes__modified`,
       JobTypes.deleted AS `JobTypes__deleted` /* MAYBE THIS COLUMN SHOULD NOT BE CALL */


       /* ALL THE IDs , TIMESTAMP , CREATEDBYUSER SHOULD NOT BE CALL IF NO PURPOSE */
       /* PUBLISH STATUS AND DELETED IS SET IN WHERE CLAUSE , CALL THIS COLUMN WILL ONLY SAME VALUE FOR ALL RETURNS */

FROM jobs Jobs

/* START JOIN SET A */

LEFT JOIN jobs_personalities JobsPersonalities ON Jobs.id = (JobsPersonalities.job_id)
LEFT JOIN personalities Personalities ON (Personalities.id = (JobsPersonalities.personality_id)
                                          AND (Personalities.deleted) IS NULL)

LEFT JOIN jobs_practical_skills JobsPracticalSkills ON Jobs.id = (JobsPracticalSkills.job_id)
LEFT JOIN practical_skills PracticalSkills ON (PracticalSkills.id = (JobsPracticalSkills.practical_skill_id)
                                               AND (PracticalSkills.deleted) IS NULL)

LEFT JOIN jobs_basic_abilities JobsBasicAbilities ON Jobs.id = (JobsBasicAbilities.job_id)
LEFT JOIN basic_abilities BasicAbilities ON (BasicAbilities.id = (JobsBasicAbilities.basic_ability_id)
                                             AND (BasicAbilities.deleted) IS NULL)

/* END JOIN SET A */


/* START JOIN SET B */

LEFT JOIN jobs_tools JobsTools ON Jobs.id = (JobsTools.job_id)
LEFT JOIN affiliates Tools ON (Tools.type = 1
                               AND Tools.id = (JobsTools.affiliate_id)
                               AND (Tools.deleted) IS NULL)

LEFT JOIN jobs_career_paths JobsCareerPaths ON Jobs.id = (JobsCareerPaths.job_id)
LEFT JOIN affiliates CareerPaths ON (CareerPaths.type = 3
                                     AND CareerPaths.id = (JobsCareerPaths.affiliate_id)
                                     AND (CareerPaths.deleted) IS NULL)

LEFT JOIN jobs_rec_qualifications JobsRecQualifications ON Jobs.id = (JobsRecQualifications.job_id)
LEFT JOIN affiliates RecQualifications ON (RecQualifications.type = 2
                                           AND RecQualifications.id = (JobsRecQualifications.affiliate_id)
                                           AND (RecQualifications.deleted) IS NULL)

LEFT JOIN jobs_req_qualifications JobsReqQualifications ON Jobs.id = (JobsReqQualifications.job_id)
LEFT JOIN affiliates ReqQualifications ON (ReqQualifications.type = 2
                                           AND ReqQualifications.id = (JobsReqQualifications.affiliate_id)
                                           AND (ReqQualifications.deleted) IS NULL)

/* END JOIN SET B */




/* START JOIN SET C */
/* BEST PRACTICE TO PLACE INNER JOIN BEFORE OUTER(LEFT/RIGHT) JOIN SO THE FILTERING WILL BE MORE SMALLER 
   ALTHOUGH MOST OF TIME OPTIMIZER WILL HELP ARRANGE THE EXECUTAION PLAN */

INNER JOIN job_categories JobCategories ON (JobCategories.id = (Jobs.job_category_id)
                                            AND (JobCategories.deleted) IS NULL)

INNER JOIN job_types JobTypes ON (JobTypes.id = (Jobs.job_type_id)
                                  AND (JobTypes.deleted) IS NULL)

/* END JOIN SET C */


/* TOO MANY WILDCARD THAT CAUSE FULL TABLE SCAN AND MAJOR PERFORMANCE , SHOULD BE REDUCE IF POSSIBLE */                                
WHERE ((JobCategories.name LIKE '%キャビンアテンダント%'
        OR JobTypes.name LIKE '%キャビンアテンダント%'
        OR Jobs.name LIKE '%キャビンアテンダント%'
        OR Jobs.description LIKE '%キャビンアテンダント%'
        OR Jobs.detail LIKE '%キャビンアテンダント%'
        OR Jobs.business_skill LIKE '%キャビンアテンダント%'
        OR Jobs.knowledge LIKE '%キャビンアテンダント%'
        OR Jobs.location LIKE '%キャビンアテンダント%'
        OR Jobs.activity LIKE '%キャビンアテンダント%'
        OR Jobs.salary_statistic_group LIKE '%キャビンアテンダント%'
        OR Jobs.salary_range_remarks LIKE '%キャビンアテンダント%'
        OR Jobs.restriction LIKE '%キャビンアテンダント%'
        OR Jobs.remarks LIKE '%キャビンアテンダント%'
        OR Personalities.name LIKE '%キャビンアテンダント%'
        OR PracticalSkills.name LIKE '%キャビンアテンダント%'
        OR BasicAbilities.name LIKE '%キャビンアテンダント%'
        OR Tools.name LIKE '%キャビンアテンダント%'
        OR CareerPaths.name LIKE '%キャビンアテンダント%'
        OR RecQualifications.name LIKE '%キャビンアテンダント%'
        OR ReqQualifications.name LIKE '%キャビンアテンダント%')
       AND publish_status = 1
       AND (Jobs.deleted) IS NULL)
GROUP BY Jobs.id
ORDER BY Jobs.sort_order DESC,
         Jobs.id DESC
LIMIT 50
OFFSET 0


```


ORIGINAL SCHEMA :

![schemaori](/images/schemaori.png)


RESTRURCTURE SCHEMA :

![schemasuggest](/images/schemasuggest.png)



RESTRURCTURE QUERY :

```
SELECT Jobs.id AS `Jobs__id`,
       Jobs.name AS `Jobs__name`,
       Jobs.media_id AS `Jobs__media_id`,
       Jobs.job_category_id AS `Jobs__job_category_id`,
       Jobs.job_type_id AS `Jobs__job_type_id`,
       Jobs.description AS `Jobs__description`,
       Jobs.detail AS `Jobs__detail`,
       Jobs.business_skill AS `Jobs__business_skill`,
       Jobs.knowledge AS `Jobs__knowledge`,
       Jobs.location AS `Jobs__location`,
       Jobs.activity AS `Jobs__activity`,
       Jobs.academic_degree_doctor AS `Jobs__academic_degree_doctor`,
       Jobs.academic_degree_master AS `Jobs__academic_degree_master`,
       Jobs.academic_degree_professional AS `Jobs__academic_degree_professional`,
       Jobs.academic_degree_bachelor AS `Jobs__academic_degree_bachelor`,
       Jobs.salary_statistic_group AS `Jobs__salary_statistic_group`,
       Jobs.salary_range_first_year AS `Jobs__salary_range_first_year`,
       Jobs.salary_range_average AS `Jobs__salary_range_average`,
       Jobs.salary_range_remarks AS `Jobs__salary_range_remarks`,
       Jobs.restriction AS `Jobs__restriction`,
       Jobs.estimated_total_workers AS `Jobs__estimated_total_workers`,
       Jobs.remarks AS `Jobs__remarks`,
       Jobs.url AS `Jobs__url`,
       Jobs.seo_description AS `Jobs__seo_description`,
       Jobs.seo_keywords AS `Jobs__seo_keywords`,
       Jobs.sort_order AS `Jobs__sort_order`,
       Jobs.publish_status AS `Jobs__publish_status`,
       Jobs.version AS `Jobs__version`,
       Jobs.created_by AS `Jobs__created_by`,
       Jobs.created AS `Jobs__created`,
       Jobs.modified AS `Jobs__modified`,
       Jobs.deleted AS `Jobs__deleted`,
       JobCategories.id AS `JobCategories__id`,
       JobCategories.name AS `JobCategories__name`,
       JobCategories.sort_order AS `JobCategories__sort_order`,
       JobCategories.created_by AS `JobCategories__created_by`,
       JobCategories.created AS `JobCategories__created`,
       JobCategories.modified AS `JobCategories__modified`,
       JobCategories.deleted AS `JobCategories__deleted`,
       JobTypes.id AS `JobTypes__id`,
       JobTypes.name AS `JobTypes__name`,
       JobTypes.job_category_id AS `JobTypes__job_category_id`,
       JobTypes.sort_order AS `JobTypes__sort_order`,
       JobTypes.created_by AS `JobTypes__created_by`,
       JobTypes.created AS `JobTypes__created`,
       JobTypes.modified AS `JobTypes__modified`,
       JobTypes.deleted AS `JobTypes__deleted`
FROM jobs Jobs
INNER JOIN job_categories JobCategories ON (JobCategories.id = (Jobs.job_category_id)
                                            AND (JobCategories.deleted) IS NULL)
INNER JOIN job_types JobTypes ON (JobTypes.id = (Jobs.job_type_id)
                                  AND (JobTypes.deleted) IS NULL)
LEFT JOIN
  (SELECT jobs_personalities.job_id
   FROM jobs_personalities
   INNER JOIN personalities ON (personalities.id = (jobs_personalities.personality_id)
                                AND (personalities.deleted) IS NULL)
   WHERE (name) LIKE '%name22%') JobsPersonalities ON Jobs.id = (JobsPersonalities.job_id)
LEFT JOIN
  (SELECT jobs_practical_skills.job_id
   FROM jobs_practical_skills
   INNER JOIN practical_skills ON (practical_skills.id = (jobs_practical_skills.practical_skill_id)
                                   AND (practical_skills.deleted) IS NULL)
   WHERE (name) LIKE '%name22%') JobsPracticalSkills ON Jobs.id = (JobsPracticalSkills.job_id)
LEFT JOIN
  (SELECT jobs_basic_abilities.job_id
   FROM jobs_basic_abilities
   INNER JOIN basic_abilities ON (basic_abilities.id = (jobs_basic_abilities.basic_ability_id)
                                  AND (basic_abilities.deleted) IS NULL)
   WHERE (name) LIKE '%name22%') JobsBasicAbilities ON Jobs.id = (JobsBasicAbilities.job_id)
LEFT JOIN
  (SELECT jobs_tools.job_id
   FROM jobs_tools
   INNER JOIN affiliates ON (affiliates.id = (jobs_tools.affiliate_id)
                             AND (affiliates.deleted) IS NULL
                             AND (affiliates.type) = '1'
                             AND (affiliates.name) LIKE '%name22%')) JobsTools ON Jobs.id = (JobsTools.job_id)
LEFT JOIN
  (SELECT jobs_career_paths.job_id
   FROM jobs_career_paths
   INNER JOIN affiliates ON (affiliates.id = (jobs_career_paths.affiliate_id)
                             AND (affiliates.deleted) IS NULL
                             AND (affiliates.type) = '3'
                             AND (affiliates.name) LIKE '%name22%')) JobsCareerPaths ON Jobs.id = (JobsCareerPaths.job_id)
LEFT JOIN
  (SELECT jobs_rec_qualifications.job_id
   FROM jobs_rec_qualifications
   INNER JOIN affiliates ON (affiliates.id = (jobs_rec_qualifications.affiliate_id)
                             AND (affiliates.deleted) IS NULL
                             AND (affiliates.type) = '2'
                             AND (affiliates.name) LIKE '%name22%')) JobsRecQualifications ON Jobs.id = (JobsRecQualifications.job_id)
LEFT JOIN
  (SELECT jobs_req_qualifications.job_id
   FROM jobs_req_qualifications
   INNER JOIN affiliates ON (affiliates.id = (jobs_req_qualifications.affiliate_id)
                             AND (affiliates.deleted) IS NULL
                             AND (affiliates.type) = '2'
                             AND (affiliates.name) LIKE '%name22%')) JobsReqQualifications ON Jobs.id = (JobsReqQualifications.job_id)
WHERE ((JobCategories.name LIKE '%name22%'
        OR JobTypes.name LIKE '%name22%'
        OR Jobs.name LIKE '%name22%'
        OR Jobs.description LIKE '%name22%'
        OR Jobs.detail LIKE '%name22%'
        OR Jobs.business_skill LIKE '%name22%'
        OR Jobs.knowledge LIKE '%name22%'
        OR Jobs.location LIKE '%name22%'
        OR Jobs.activity LIKE '%name22%'
        OR Jobs.salary_statistic_group LIKE '%name22%'
        OR Jobs.salary_range_remarks LIKE '%name22%'
        OR Jobs.restriction LIKE '%name22%'
        OR Jobs.remarks LIKE '%name22%'
        
       /* if left join is succeed , it will return job_id with value */        
        
        OR JobsTools.job_id IS NOT NULL
        OR JobsCareerPaths.job_id IS NOT NULL
        OR JobsRecQualifications.job_id IS NOT NULL
        OR JobsReqQualifications.job_id IS NOT NULL
        OR JobsPersonalities.job_id IS NOT NULL
        OR JobsPracticalSkills.job_id IS NOT NULL
        OR JobsBasicAbilities.job_id IS NOT NULL)
       AND publish_status = 1
       AND (Jobs.deleted) IS NULL)
GROUP BY Jobs.id
ORDER BY Jobs.sort_order DESC,
         Jobs.id DESC
LIMIT 50
OFFSET 0


```

I created my own dummy database and the export is sqltune.mysql : https://github.com/ulhakim/sqltune/blob/main/dummydatabasecreate/sqltune.sql

I change the search keyword from 'キャビンアテンダント' to 'name22' for my test environment(sqltune.mysql).

Other considerable method in faster execution:

1. Store Procedure , this is big point.
2. database structure re-build,  from what i can see, the data delete is not delete from table, just label delete in the column deleted. this might cause a heavy operation in future if there are too much deleted data.


