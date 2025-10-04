# Deploying Fullstack Web App to AWS & integrating Dynatrace

## Dynatrace Integration Guide 🛠️

Key Dynatrace components we'll use:
- **OneAgent** + **Hosts Classic app** - EC2 backend monitoring
- **Agentless Real User Monitoring (RUM)** - detailed user experience monitoring & analytics
- **Session Segmentation** - User experience *Aggregated* analytics
- **Digital experience -> Web** + **Frontend app** - real user monitoring (RUM)
- **Dashboards** - putting together customised reports & data views

### Pre-requisites ✅
1. Dynatrace account : Trial/Regular
2. Completed AWS Web App deployment
3. Basic understanding of "Inspect" functionality in modern browsers

### Server-side monitoring & Observability (EC2) 🤖
1. Dynatrace -> Apps -> "Deploy OneAgent" -> Follow the guide (don't forget to save the token) on your EC2 instance as root (```sudo su -```) -> Show deployment status (new record should pop up there). 
Dynatrace app you'll be taken to by default (after deploying OneAgent) is "Infrastructure & Operations": a newer development forked from "Hosts classic"
2. EC2: restart Node process with:
   ```shell
   pm2 delete all
   pm2 start ecosystem.config.js
   ```
This way OneAgent will get access to the insides of your Web App's performance

3. Apps -> Hosts Classic -> Explore our EC2 instance's performance metrics
    Enable "Disk analytics" extension (Add to environment) + "Net Tracer Traffic monitoring" in Hosts Classic
4. Check out updated server monitoring experience with "Infrastructure & Monitoring" app

### Real User Monitoring (RUM) 👫
1. Dynatrace -> Apps -> "Agentless Real User Monitoring" -> "inventorymanagement" Add web application
2. Copy JavaScript tag -> Modify Dynatrace's javascript contents: 
   * "crossorigin" ->> "crossOrigin" 
   * add "strategy="afterInteractive"" before "src=..."
   * "<script...>" & "</script>" ->> "<Script...>" & "</Script>"
   * 
3. Add modified javascript to client/src/app/layout.tsx
   
The code snippet should go between: "<head>HERE</head>" after "<html lang="en">" and before "<body
        className=..."

4. Commit & push changes to Github
```shell
git add client/src/app/.
git commit -m"integrated Dynatrace agentless rum"
git push origin master
```
5. Make sure that the new Deployment goes through in AWS Amplify & Web App keeps working normally.
Open your Web App's URL in different browsers, from your phone, share with friends & family: we want to collect different user sessions to explore later!

6. Checkout out Dynatrace: Agentless real user monitoring -> Monitored web applications -> View application (a tiny button)
7. Your Web App's performance analysis now lives under "Dynatrace -> Apps -> Web"
