# Pipeline
- **image**: which Docker container use to run the job
- **artifacts**: position save to share with other 
- **stage** - **stages**: The most common pipeline configurations group jobs into stages. 
	- Jobs in the same stage can run in parallel, while jobs in later stages wait for jobs in earlier stages to complete. 
	- If a job fails, the whole stage is considered failed and jobs in later stages do not start running.
	- 