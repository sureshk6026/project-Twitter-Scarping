import snscrape.modules.twitter as sntwitter
import pandas as pd
import pymongo
from pymongo import MongoClient
import json
from pprint import pprint
import datetime
import streamlit as st
import numpy as np

st.title("Scarping document from twitter")
attributes_container = []
st.text_input(Tweet=input("enter"))
for i, tweet in enumerate(sntwitter.TwitterSearchScraper(Tweet).get_items()):
    if i > 1500:
        break
    attributes_container.append(
        [tweet.date, tweet.id, tweet.url, tweet.quotedTweet, tweet.content, tweet.user, tweet.replyCount,
         tweet.retweetedTweet, tweet.likeCount, tweet.sourceLabel, tweet.lang])

df = pd.DataFrame(attributes_container,
                  columns=["Date", "ID", "URL", "Tweet", "Content", "User Name", "replycount", "Retweeted",
                           "Number of Likes", "Source of Tweet", "language"])
#df['Date Created'] = pd.to_datetime(df['Date Created'])
#print(df.dtypes)
df['Date']= pd.to_datetime(df['Date'], errors='coerce')
df['Date']=df['Date'].dt.strftime('%Y-%m-%d')
records=json.loads(df.T.to_json()).values()
da = df.to_dict('records')
def date(a,b):
    client = pymongo.MongoClient("mongodb://localhost:27017")
    db = client["SureshKumar"]
    collection = db["New collection"]
    WWE = df.to_json(orient="records", date_format='iso')
    parsed = json.loads(WWE)
    mongoImp = collection.insert_many(parsed)
for each in collection.find():
    print(each)
query={"Date":{"$in:[a,b]}}
y=collection.find(query, {'Content': 1, '_id': 0}).limit(10)
for e in y:
    print(e)
st.text.input(a=input("enter the start date"))
st.text.input(b=input("enter the end date"))
date(a,b)
st.text_area(print(e))
st.download_button(doc=e.to_csv())
st.download_button(db.to_csv())

# project-Twitter-Scarping
Scarping document from twitter 
