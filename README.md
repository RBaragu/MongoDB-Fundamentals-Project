# MongoDB-Fundamentals-Project

## Introduction
As a Data engineer working for a firm. You have been given a dataset that contains emails data
and you need to perform the following tasks:

[download the dataset here] (https://bit.ly/3I4S1uP)

- Import it into Mongo using `mongoimport`
```
mongoimport --db enron --collection emails --file enron.json
```
- Change the date data from a string 
 ```
 new Date("2000-08-23 02:16:00-07:00")
 
 ```
 - Convert the whole date data by iterating through it 
 ```
 db.enron.find().forEach(function(doc){doc.date = new Date(doc.date);db.enron.save(doc)});
 
 ```
 
 - Tasks
 1. Group by the sender.
 ```
 db.enron.aggregate({$group:{_id:'$sender'}})
 
 ```
 
 ```
 project> db.enron.aggregate({$group:{_id:'$sender'}})
[
  { _id: 'amccarty@houston.org' },
  { _id: 'hreasoner@velaw.com' },
  { _id: 'lavergne_schwender@co.harris.tx.us' },
  { _id: 'mcgarrymusic@yahoo.com' },
  { _id: 'lmilowe@scmc.org' },
  { _id: 'sean@witheveryidlehour.com' },
  { _id: 'lcoleygray@sbcglobal.net' },
  { _id: 'maokotani@aol.com' },
  { _id: 'arader1016@msn.com' },
  { _id: 'mgiese@pcbackup.cc' },
  { _id: 'msaade@uwtgc.org' },
  { _id: 'marl552@aol.com' },
  { _id: 'olympianmk@al.com' },
  { _id: 'dewey@deweyballantine.com' },
  { _id: 'rajesh.sarkar@mphasis.com' },
  { _id: 'betsy.mcquaid@ucop.edu' },
  { _id: 'lregopoulos@aei.org' },
  { _id: 'zach.moring@enron.com' },
  { _id: 'winifred.isaac@enron.com' },
  { _id: 'hhabicht@getf.org' }
]
Type "it" for more

```
2. List all the unique senders.

```
db.enron.aggregate({$group:{_id:{sender:'$sender'}}},{$project:{'_id.sender':true}})

```
```
project> db.enron.aggregate({$group:{_id:{sender:'$sender'}}},{$project:{'_id.sender':true}})
[
  { _id: { sender: 'cynthia.x.rutherford@kp.org' } },
  { _id: { sender: 'tinah53374@aol.com' } },
  { _id: { sender: 'denisea38@yahoo.com' } },
  { _id: { sender: 'muncheysmom@netscape.net' } },
  { _id: { sender: 'helen.tunley@gs.com' } },
  { _id: { sender: 'michael.mann@enron.com' } },
  { _id: { sender: 'cbt@medicine.wisc.edu' } },
  { _id: { sender: 'lizard_ar@yahoo.com' } },
  { _id: { sender: 'agoldschmidt@earthlink.net' } },
  { _id: { sender: 'legsalad@hotmail.com' } },
  { _id: { sender: 'leila@globalexchange.org' } },
  { _id: { sender: 'hswygert@howard.edu' } },
  { _id: { sender: 'alexis415@yahoo.com' } },
  { _id: { sender: 'ecockrel@cockrell.com' } },
  { _id: { sender: 'jashford@jashford.com' } },
  { _id: { sender: 'eleanor.best@bc.edu' } },
  { _id: { sender: 'info@enron.com' } },
  { _id: { sender: 'gsattler@geosociety.org' } },
  { _id: { sender: 'kitty@salk.edu' } },
  { _id: { sender: 'ecohan@rei.com' } }
]
Type "it" for more

```

3. Count all the unique senders

```
db.enron.aggregate({$group:{_id:'$sender'}},{$group:{_id:1,count:{$sum:1}}})

```

```
project> db.enron.aggregate({$group:{_id:'$sender'}},{$group:{_id:1,count:{$sum:1}}})
[ { _id: 1, count: 1985 } ]

```

4. Group by sender and count to find out which email address sent the most emails.

```
db.enron.aggregate({$group:{_id:{sender:"$sender",sendreceive:{folder:"_sent"}},count:{$sum:1}}},{$sort:{count:-1}},{$project:{'_id.sender':true,'_id.sendreceive':true,count:true}})

```

```
project> db.enron.aggregate({$group:{_id:{sender:"$sender",sendreceive:{folder:"_sent"}},count:{$sum:1}}},{$sort:{count:-1}},{$project:{'_id.sender':true,'_id.sendreceive':true,count:true}})
[
  {
    _id: {
      sender: 'rosalee.fleming@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 596
  },
  {
    _id: {
      sender: 'brown_mary_jo@lilly.com',
      sendreceive: { folder: '_sent' }
    },
    count: 56
  },
  {
    _id: {
      sender: 'leonardo.pacheco@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 52
  },
  {
    _id: {
      sender: 'savont@email.msn.com',
      sendreceive: { folder: '_sent' }
project>
    count: 44
  },
  {
    _id: {
      sender: 'tori.wells@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 41
  },
  {
    _id: {
      sender: 'no.address@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 37
  },
  {
    _id: {
      sender: 'elizabeth.davis@compaq.com',
      sendreceive: { folder: '_sent' }
    },
    count: 34
  },
  {
    _id: {
      sender: 'elizabeth.lay@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 33
  },
  {
    _id: {
      sender: 'katherine.brown@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 32
  },
  {
    _id: { sender: 'svarga@kudlow.com', sendreceive: { folder: '_sent' } },
    count: 30
  },
  {
    _id: { sender: 'lizard_ar@yahoo.com', sendreceive: { folder: '_sent' } },
    count: 30
  },
  {
    _id: { sender: 'mrslinda@lplpi.com', sendreceive: { folder: '_sent' } },
    count: 28
  },
  {
    _id: {
      sender: 'karen.denne@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 26
  },
  {
    _id: {
      sender: 'rob.bradley@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 22
  },
  {
    _id: {
      sender: 'terrie.james@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 21
  },
  {
    _id: {
      sender: 'jeffrey.garten@yale.edu',
      sendreceive: { folder: '_sent' }
    },
    count: 21
  },
  {
    _id: {
      sender: 'enron.announcements@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 20
  },
  {
    _id: {
      sender: 'joe.hillings@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 19
  },
  {
    _id: { sender: 'dbrock@howard.edu', sendreceive: { folder: '_sent' } },
    count: 19
  },
  {
    _id: { sender: 'shea_dugger@i2.com', sendreceive: { folder: '_sent' } },
    count: 18
  }
]
Type "it" for more

```

5. Rank the senders in order or emails sent.
```
db.enron.aggregate({$group:{_id:{sender:"$sender",sendreceive:{folder:"_sent"}},count:{$sum:1}}},{$sort:{count:-1}},{$project:{'_id.sender':true,'_id.sendreceive':true,count:true}})

```

```
project> db.enron.aggregate({$group:{_id:{sender:"$sender",sendreceive:{folder:"_sent"}},count:{$sum:1}}},{$sort:{count:-1}},{$project:{'_id.sender':true,'_id.sendreceive':true,count:true}})
[
  {
    _id: {
      sender: 'rosalee.fleming@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 596
  },
  {
    _id: {
      sender: 'brown_mary_jo@lilly.com',
      sendreceive: { folder: '_sent' }
    },
    count: 56
  },
  {
    _id: {
      sender: 'leonardo.pacheco@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 52
  },
  {
    _id: {
      sender: 'savont@email.msn.com',
      sendreceive: { folder: '_sent' }
project>
    count: 44
  },
  {
    _id: {
      sender: 'tori.wells@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 41
  },
  {
    _id: {
      sender: 'no.address@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 37
  },
  {
    _id: {
      sender: 'elizabeth.davis@compaq.com',
      sendreceive: { folder: '_sent' }
    },
    count: 34
  },
  {
    _id: {
      sender: 'elizabeth.lay@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 33
  },
  {
    _id: {
      sender: 'katherine.brown@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 32
  },
  {
    _id: { sender: 'svarga@kudlow.com', sendreceive: { folder: '_sent' } },
    count: 30
  },
  {
    _id: { sender: 'lizard_ar@yahoo.com', sendreceive: { folder: '_sent' } },
    count: 30
  },
  {
    _id: { sender: 'mrslinda@lplpi.com', sendreceive: { folder: '_sent' } },
    count: 28
  },
  {
    _id: {
      sender: 'karen.denne@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 26
  },
  {
    _id: {
      sender: 'rob.bradley@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 22
  },
  {
    _id: {
      sender: 'terrie.james@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 21
  },
  {
    _id: {
      sender: 'jeffrey.garten@yale.edu',
      sendreceive: { folder: '_sent' }
    },
    count: 21
  },
  {
    _id: {
      sender: 'enron.announcements@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 20
  },
  {
    _id: {
      sender: 'joe.hillings@enron.com',
      sendreceive: { folder: '_sent' }
    },
    count: 19
  },
  {
    _id: { sender: 'dbrock@howard.edu', sendreceive: { folder: '_sent' } },
    count: 19
  },
  {
    _id: { sender: 'shea_dugger@i2.com', sendreceive: { folder: '_sent' } },
    count: 18
  }
]
Type "it" for more

```

6. Rank the senders in order or emails sent + emails received.
```
db.enron.aggregate({$group:{_id:{sender:"$sender",sendreceive:"$folder"},count:{$sum:1}}},{$sort:{count:-1}},{$project:{'_id.sender':true,'_id.sendreceive':true,count:true}})

```

```
project> db.enron.aggregate({$group:{_id:{sender:"$sender",sendreceive:"$folder"},count:{$sum:1}}},{$sort:{count:-1}},{$project:{'_id.sender':true,'_id.sendreceive':true,cocount:true}})
[ },
  {
    _id: {
      sender: 'rosalee.fleming@enron.com',
      sendreceive: 'all_documents't' }
    },
    count: 252
  },
  {
    _id: { sender: 'rosalee.fleming@enron.com', sendreceive: '_sent' },
    count: 246'no.address@enron.com',
  },  sendreceive: { folder: '_sent' }
  { },
    _id: { 37
      sender: 'rosalee.fleming@enron.com',
      sendreceive: 'discussion_threads'
    },d: {
    count: 93 'elizabeth.davis@compaq.com',
  },  sendreceive: { folder: '_sent' }
  { },
    _id: { sender: 'svarga@kudlow.com', sendreceive: 'inbox' },
    count: 30
  },
  { _id: {
    _id: { sender: 'no.address@enron.com', sendreceive: 'inbox' },
    count: 28eive: { folder: '_sent' }
  },},
  { count: 33
    _id: {
      sender: 'leonardo.pacheco@enron.com',
      sendreceive: 'all_documents'
    },sender: 'katherine.brown@enron.com',
    count: 26eive: { folder: '_sent' }
  },},
  
  ```
  7. Create a timeline that shows the number of emails by day
  
  ```
  db.enron.aggregate({$project:{year:{$year:"$date"},month:{$month:"$date"},dayOfMonth:{$dayOfMonth:"$date"}}},{$group:{_id:{year:'$year',month:'$month',dayOfMonth:'$dayOfMonth'},count:{$sum:1}}},{$sort:{count:-1}})
  
  ```
  
  ```
  project> db.enron.aggregate({$project:{year:{$year:"$date"},month:{$month:"$date"},dayOfMonth:{$dayOfMonth:"$date"}}},{$group:{_id:{year:'$year',month:'$month',dayOfMonth:'$dayOfMonth'},count:{$sum:1}}},{$sort:{count:-1}})
[
  { _id: { year: 2002, month: 1, dayOfMonth: 30 }, count: 1120 },
  { _id: { year: 2001, month: 11, dayOfMonth: 14 }, count: 42 },
  { _id: { year: 2000, month: 10, dayOfMonth: 13 }, count: 39 },
  { _id: { year: 2000, month: 10, dayOfMonth: 12 }, count: 39 },
  { _id: { year: 2000, month: 11, dayOfMonth: 20 }, count: 38 },
  { _id: { year: 2000, month: 10, dayOfMonth: 31 }, count: 38 },
  { _id: { year: 2000, month: 9, dayOfMonth: 21 }, count: 37 },
  { _id: { year: 2000, month: 10, dayOfMonth: 5 }, count: 36 },
  { _id: { year: 2000, month: 11, dayOfMonth: 15 }, count: 36 },
  { _id: { year: 2000, month: 10, dayOfMonth: 10 }, count: 36 },
  { _id: { year: 2000, month: 11, dayOfMonth: 9 }, count: 35 },
  { _id: { year: 2000, month: 11, dayOfMonth: 28 }, count: 35 },
  { _id: { year: 2000, month: 10, dayOfMonth: 11 }, count: 34 },
  { _id: { year: 2000, month: 10, dayOfMonth: 6 }, count: 33 },
  { _id: { year: 2000, month: 11, dayOfMonth: 30 }, count: 33 },
  { _id: { year: 2000, month: 11, dayOfMonth: 6 }, count: 31 },
  { _id: { year: 2000, month: 9, dayOfMonth: 8 }, count: 31 },
  { _id: { year: 2001, month: 10, dayOfMonth: 24 }, count: 31 },
  { _id: { year: 2000, month: 12, dayOfMonth: 13 }, count: 31 },
  { _id: { year: 2000, month: 12, dayOfMonth: 6 }, count: 29 }
]
Type "it" for more

```

8. Create a filter that shows the number of emails by day, sent by people who had sent 10
or more emails.
    - First, prep the data. The date will be stored as a string and you would need to
convert it to an actual date.
    - We can split the data using a script like this one. See if you can see what it is
doing:

```
db.startups.find({}).forEach(function(startup) {
if (startup.tag_list && startup.tag_list.split) {
startup.tag_list =
startup.tag_list.split(',').map(function(a){return a.trim()});
} else {
startup.tag_list = [];
}
print(startup.tag_list);
db.startups.save(startup)
})

```

```
db.enron.aggregate({$group:{_id:'$sender',count:{$sum:1},entry:{$push:{date:"$date"}}}},{$sort:{count:-1}},{$match:{count:{$gt:10}}},{$group:{_id:'$entry.date'}},{"$unwind":{"path": "$_id"}},{$project:{year:{$year:"$_id"},month:{$month:"$_id"},dayOfMonth:{$dayOfMonth:"$_id"}}},{$group:{_id:{year:'$year',month:'$month',dayOfMonth:'$dayOfMonth'},count:{$sum:1}}},{$sort:{count:-1}})

```
```
project> db.enron.aggregate({$group:{_id:'$sender',count:{$sum:1},entry:{$push:{date:"$date"}}}},{$sort:{count:-1}},{$match:{count:{$gt:10}}},{$group:{_id:'$entry.date'}},{"$unwind":{"path": "$_id"}},{$project:{year:{$year:"$_id"},month:{$month:"$_id"},dayOfMonth:{$dayOfMonth:"$_id"}}},{$group:{_id:{year:'$year',month:'$month',dayOfMonth:'$dayOfMonth'},count:{$sum:1}}},{$sort:{count:-1}})
[
  { _id: { year: 2000, month: 11, dayOfMonth: 20 }, count: 22 },
  { _id: { year: 2000, month: 10, dayOfMonth: 12 }, count: 21 },
  { _id: { year: 2000, month: 9, dayOfMonth: 8 }, count: 19 },
  { _id: { year: 2000, month: 10, dayOfMonth: 13 }, count: 19 },
  { _id: { year: 2000, month: 5, dayOfMonth: 9 }, count: 19 },
  { _id: { year: 2000, month: 11, dayOfMonth: 9 }, count: 17 },
  { _id: { year: 2001, month: 3, dayOfMonth: 23 }, count: 17 },
  { _id: { year: 2000, month: 11, dayOfMonth: 6 }, count: 16 },
  { _id: { year: 2000, month: 10, dayOfMonth: 11 }, count: 16 },
  { _id: { year: 2000, month: 10, dayOfMonth: 31 }, count: 16 },
  { _id: { year: 2000, month: 10, dayOfMonth: 3 }, count: 15 },
  { _id: { year: 2000, month: 10, dayOfMonth: 25 }, count: 14 },
  { _id: { year: 2000, month: 11, dayOfMonth: 15 }, count: 14 },
  { _id: { year: 2001, month: 5, dayOfMonth: 1 }, count: 13 },
  { _id: { year: 2001, month: 3, dayOfMonth: 19 }, count: 13 },
  { _id: { year: 2000, month: 11, dayOfMonth: 27 }, count: 13 },
  { _id: { year: 2000, month: 11, dayOfMonth: 10 }, count: 13 },
  { _id: { year: 2000, month: 9, dayOfMonth: 25 }, count: 12 },
  { _id: { year: 2000, month: 11, dayOfMonth: 21 }, count: 12 },
  { _id: { year: 2000, month: 11, dayOfMonth: 8 }, count: 12 }
]
Type "it" for more

```


