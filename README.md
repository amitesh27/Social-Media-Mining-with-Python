# Social-Media-Mining-with-Python


from tweepy.streaming import StreamListener
from tweepy import OAuthHandler
from tweepy import Stream
import csv
import json
import datetime

#Variables that contains the user credentials to access Twitter API
access_token = "872442140944719872-wHUNUiM4oZDwzUUAsOFEOWxMhUCtYBy"
access_token_secret = "nkVDPWKd5RVYW12yKHKMzaBlBp11Fw0LEXLzID8ESD0Cd"
consumer_key = "jL7h4jSaDthoEUTxT8f37nUWD"
consumer_secret = "ewxCI1JfNAsm1hrhhoGyMAghz6LcAx4n9jUzkbK4Tdo6k9TQLn"


#This is a basic listener that just prints received tweets to stdout.

class listener (StreamListener):

    def on_data(self, raw_data):

        data = json.loads(raw_data)
        print data.keys()
        tweet = data['text'].encode("utf-8")
        tweet_id = data['id']
        time_tweet = data['timestamp_ms']
        date = datetime.datetime.fromtimestamp(int(time_tweet) / 1000)
        new_date = str(date).split(" ") [0]
        print new_date
        user_id = data['user']['id']
        with  open('altcoinDB.csv','ab') as csvfile:
            myfile = csv.writer(csvfile)
            myfile.writerow([tweet_id,new_date,tweet,user_id])



        return True

    def on_error(self, status_code):
        print status_code

auth = OAuthHandler(consumer_key,consumer_secret)
auth.set_access_token(access_token,access_token_secret)
twitterStream = Stream(auth,listener())
twitterStream.filter(track=["altcoin"])
