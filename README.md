![](https://challengepost-s3-challengepost.netdna-ssl.com/photos/production/software_photos/000/610/781/datas/gallery.jpg)

# Summarize
As a group, we were looking for a way to help people in sort of an unexpected way. After thinking about it for quite some time, we realized that a large majority of students in college have never developed the proper note taking habits that are required to get every single important piece that a professor may talk about. Some facilities on campus offer note taking services; however, this is inconvenient to a certain point because the student has to rely on someone else for them to succeed in class. This product completely removes any issue that students may have with taking meaningful notes in class, and allows them to keep track of every important detail that their professor touches on.

## What it does
TL;DR: our app records what a professor says and organizes a study guide based on the most important parts of the lecture.

Our mobile app utilizes voice-to-text to track what a professor is saying in real-time. After a class has finished and all of the speaking has been converted to a manipulable data type, we send the words that were just spoken to a flask back-end that was deployed on Heroku. The server processes the words using artificial intelligence to predict where the punctuation should be. The API that is being used, to create our tags (highest occurring words) and the summary of the text, requires punctuation which means that the only way to place punctuation was by following a heuristic model to estimate where periods, exclamation points and question marks need to go. After the words were formatted in a proper grammatical way, the most important points of the text were organized into a document to be viewed in the mobile application or on the web app that was created specifically to support the crowd sourcing of different lecture notes.

This project was a submission to HackFSU 5; for more details click [here](https://devpost.com/software/notif-ai).

![](https://challengepost-s3-challengepost.netdna-ssl.com/photos/production/software_photos/000/610/798/datas/gallery.jpg)
![](https://challengepost-s3-challengepost.netdna-ssl.com/photos/production/software_photos/000/610/796/datas/gallery.jpg)
![](https://challengepost-s3-challengepost.netdna-ssl.com/photos/production/software_photos/000/610/797/datas/gallery.jpg)
