# spec
Autonomous cloud fabric specification

Requirements
Environments
For a successful Captain/ACM deployment you will require Dev, Staging and Production environments. Each one should contain Blue/Green instances to facilitate the automation features.
Dev
The Dev environment will consist of a single instance. 
To be promoted form Dev to Staging code will first have to pass performance and functional tests. A successful test will automatically trigger the promotion to staging. This will be queued if tests are currently running in staging. 
Staging
The Staging environment will consist of Blue/Green. New deployments promoted to Staging will be deployed on the oldest version. As soon as it is deployed E2E performance tests will be triggered. During these tests all promotions from Dev will be queued. If the tests are successful the build will remain, if unsuccessful the build will be rolled back.
Production
The Production environment will also consist of Blue/Green. New deployments promoted to Production will be deployed on the oldest version. Istio will be used to test these in a canary deployment fashion. If a problem is detected then it will be rolled back/alter istio

Application
The application needs to run micro services for a successful implementation. There also needs to be defined KPIs for the app to determine the test success factors
Workflow

TBC
Logic
The entire pipeline should be as automated as possible. The approach we have taken is that the developers can simply issue a pull request and that will trigger our pipeline. If tests are successful then the new version will remain but they fail and the stage is >= Staging then it will automatically be rolled back. We don’t want to automatically roll back Dev as issues may need to be properly investigated/diagnosed.

Istio Settings in staging
Blue is always stable and green will be the latest release candidate. In staging the Istio settings will be adjusted based upon the tests being run. For the performance tests the traffic will be split 50:50 to compare the new release to the latest stable release. This can be compared in the monitoring solution side by side. 
For the E2E tests, traffic will be split 0:100 to ensure all E2E tests are executed against the new release on green.
For the auto-remediation tests the traffic will be split to 100:0
