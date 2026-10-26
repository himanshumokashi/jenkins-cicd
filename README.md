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

Before publishing:
- Replace the example GitHub link (https://github.com/) with your real profile/repository.
- Replace hello@example.com with your real contact email.
- The website labels the Jenkins-to-EC2 pipeline as in progress, not completed.
- Google Fonts are loaded from the internet; the page still uses fallback fonts if offline.
