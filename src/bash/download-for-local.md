#!/bin/bash

# Check wheather you have bsondump command or not
if [ `which bsondump` == "" ]; then
  echo "WARN: You don't have bsondump command. You should install mongodbd."
  exit
fi

# Make a directory to store downloaded data
mkdir -p /tmp/github-archive-data/
cd /tmp/github-archive-data/

# Download Github Archive data at 2015-01-01
# https://www.githubarchive.org/
wget http://data.githubarchive.org/2015-01-01-{0..23}.json.gz

# Download Github user data at 2015-01-29
# And arrange the data as 'github-users.json'
wget http://ghtorrent.org/downloads/users-dump.2015-01-29.tar.gz
tar zxvf users-dump.2015-01-29.tar.gz
# Replace ObjectId with null. ObjectId is used for mongoDB, not valid JSON.
bsondump dump/github/users.bson | sed -e "s/ObjectId([^)]*)/null/" > github-users.json
