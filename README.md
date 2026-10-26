DEVOPS LAUNCHPAD WEBSITE
========================

Files:
  site/index.html
  site/style.css
  site/script.js

Preview locally with your existing Docker project:
1. Copy these three files into your existing ~/jenkins-cicd/site/ folder in WSL.
2. From ~/jenkins-cicd run:
     docker build -t jenkins-cicd:v2 .
     docker rm -f jenkins-cicd-test
     docker run -d --name jenkins-cicd-test -p 8086:80 jenkins-cicd:v2
3. Open http://localhost:8086

