## Project Motivation
In an emergency time is crucial and the rescuers or an organization can be overloaded with the amount of messages
generated during a crisis. This project uses Machine Learning, in order to read those emergency messages and
categorize them into categories. Therefore, emergency services can prioritize the those messages and saving them time
 to read all the messages.

The API and frontend developed gives a glimpse of the data contained in a database and it provides the category predictions for
emergency messages as shown below.

<img src='media/project_cover.gif' width="800" height="500" />
<br>


## Project Requirements

* Python3 with an [Anaconda Distribution](https://www.anaconda.com/products/individual) (recommended)

Or
* Python 3 with the necessary requirements, open up a virtual environment and run `pip install -r requirements.txt`

## Project Structure

    .
    ├── app     
    │   ├── run.py                           # Flask file that runs app
    │   └── templates   
    │       ├── go.html                      # Classification result page of web app
    │       └── master.html                  # Main page of web app    
    ├── data                   
    │   ├── disaster_categories.csv          # Dataset including all the categories  
    │   ├── disaster_messages.csv            # Dataset including all the messages
    │   └── process_data.py                  # Data cleaning
    ├── models
    │   └── train_classifier.py              # Train ML model    
    ├── pipeline_design     
        │   ├── ETL Pipeline Preparation.ipynb   # Design of the ETL Pipeline
        │   └── ML Pipeline Preparation.ipynb    # Design of the ML Pipeline       
    └── README.md

