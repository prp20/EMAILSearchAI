# EMAILSearchAI

### What is EMAILSearch.AI?
This AI application simplifies email management by enabling users to efficiently search through their emails using natural language queries. It processes a dataset of CSV files containing detailed email attributes such as subject, body, sender, recipient, timestamp, along with a summary dataset of email threads. <br> <br>
Upon receiving a query, the application identifies relevant answers and cites the original email source, ensuring users can verify the information's authenticity. <br> <br>
By providing precise answers and traceable references, this tool enhances productivity and accuracy, offering a seamless and reliable email search experience for professionals managing large volumes of communication.<br> <br>

### Objective:
**Develop an AI application that:**
1) Streamlines email management by enabling users to efficiently search through emails using natural language queries.
2) Process email data containing attributes such as subject, body, sender, recipient, and timestamp, along with a summary of email threads.
3) Identify relevant answers to user queries and cite the original email source for verification.
4) Provide accurate and traceable responses,to enhance productivity and ensure reliable email search capabilities.

### Dataset Used:
[Email Thread Summary Dataset](https://www.kaggle.com/datasets/marawanxmamdouh/email-thread-summary-dataset/data)
The Email Thread Dataset is a kaggle dataset that comprises two primary files:<br> 
	- email_thread_details <br>
	- email_thread_summaries <br>
  which together provide a detailed and summarized view of email threads. <br><br>
  The email_thread_details file includes information on 21,684 emails, covering attributes like thread ID, subject, timestamp, sender, recipients, and email body, with recipient information accessible in CSV and Pickle formats.The email_thread_summaries file offers concise human-annotated summaries for 4,167 email threads, presenting a high-level overview of their content. <br>
  This dataset, organized into threads and emails, serves as a comprehensive resource for analysis and research, supporting English-language communication insights. <br>

### How to install
- `pip install -r requirements.txt`
- Download the dataset and extract the dataset into the local folder.
- Run the jupyter notebook.

### Design & Implementation:
The design of this application is in 4 stages: <br>
&nbsp;&nbsp;&nbsp;&nbsp;1) **Knowledge Gathering, Data Understanding and Preprocessing**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; In this stage where we <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Gather the data for the dataset. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Identify the suitable parameters that are needed by the RAG model (to be described later). <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Preprocess the data such that only the necessary data is retained and all the unnecessary data has been&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cleaned. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Create a suitable dataframe/dataset that can be used to be fed to the model which contains only the necessary columns and dataset. <br> <br>
&nbsp;&nbsp;&nbsp;&nbsp;2) **Generating Embeddings, Creating Vector Store and Storing the embeddings** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Using the data generated above, create embeddings that are suitable to be stored in a vector database. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Create a persistent database in which the generated embeddings can be stored in the form of a collection. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Create two collections where one is a cache collection and another is a main collection. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- In the cache collection store only those data that has been used before, while in the main collection store <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;all the embeddings that have been generated from the OpenAI embeddings. <br><br>
&nbsp;&nbsp;&nbsp;&nbsp;3) **Populating Cache Layer, Re-Rank the results**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Based on the query received, search the cache layer for the suitable result. If not found, search the main collection for the same.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Store the top 10 results obtained into the cache Layer by sorting the results based on their semantic score.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Once the score has been obtained, using a cross-encoder, re-rank the results such that they are more accurate. (This step is optional as &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; re-ranking is time consuming and resource intensive.) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Store the re-ranked results into the cache.<br><br>
&nbsp;&nbsp;&nbsp;&nbsp;4) **Generate User Response** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- From the re-ranked results in the cache, obtain the top 3 results and pass the same to the conversational api of chatgpt.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- This API, based on RAG, returns a suitable response containing the right suggestion for the user. The response would be in a user &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; understandable format, more like a user assistant.<br>
