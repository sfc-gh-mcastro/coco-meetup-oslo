# Oslo Meetup Script

START IN COCO CLI.

Given the dataset for a mortgage lending prediction execise lets use COCO CLI to

1. quick overview of the data
2. Create a database.schema to be used in the project
3. Upload the data in csv to the stage database.schema.stage

JUMP to COCO SNOWSIGHT.

Ok. Now we use workspace to continue experimentation.

1. **Load the data from stage database.schema.stage into a table** for easier analysis?

2. lets run a Run exploratory analysis in a python notebook.
3. Based on this data lets build a machine learning model
   1. Which model have you created?
   2. lets try with XGboost
   3. /plan
   4. **Register model to Snowflake** - Save XGBoost to Model Registry for versioning & governance
   5. lets add model explainability
   6. what can I do now?
   7. Now lets Run SQL inference 
   8. OPTIONAL: Analyze prediction performance by loan type and county to identify which segments have highest approval rates
   9. Can you summarize all the steps done in this session in a markdown file named readme.md. Also create a session with sugestion for next steps that I can use next time to continue developing this.
   10. what is the best way to start from where I stoped here?
4. SEMANTIC VIEW AND AGENT
   1. Lets create a semantic view named coco_mortage_semantic_view in coco-meetup-oslo. After that create an Snowflake Inteligence COCO_MORTAGE_DEMO_AGENT using the semantic view created. The Agent will be used to investigate about the data from the COCO-MEETUP-OSLO".PUBLIC.MORTGAGE_LENDING.