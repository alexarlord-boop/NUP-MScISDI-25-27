1. Requirements  
   1. Stakeholder interviews  
      1. Conduction of at least 3 interviews with critical service providers and bank managers  
      2. Covering of gaps and needs in a shared work document  
      3. Implementation a central knowledge base for all the teams.  
   2. Listing of a feature set  
      1. Non-functional  
      2. Functional  
   3. Prioritization of a feature set  
      1. Creation of MOST analysis artifact  
      2. Creation of SWOT analysis artifact  
      3. Creation of MoSCoW analysis artifact  
2. Design  
   1. UX  
      1. Creation 5-10 personas   
      2. Brainstorming user flows and use cases – pack them in a shared artifact for the development teams.  
      3. Cross validation them with real users, adapt and adjust assumptions for core features.  
   2. Design System  
      1. Implementation visuals for splash screens, icons, color schemes and themes – pack in a shared figma file  
      2. Building core components used across all app states – pack in a reusable Typescript UI toolkit  
   3. UI  
      1. Creation of Mockups  
      2. Creation of wireframes   
      3. Validation of wireframes with users  
      4. Creation of featured figma screens for the app

3. Development  
   1. Implementation of API  
      1. Analysis of the Domain  
      2. OpenApi specification shared in a doc  
      3. Full description of endpoints and request-response examples  
   2. Creation of the App Backend  
      1. Building of the Domain Driven Design structures and classes  
      2. Separation into modules and micro-services for teams  
      3. Implementation of the OAS  
   3. Creation of the App Frontend  
      1. Creation of the mock data for OAS  
      2. Wiring GUI client with the OAS mock endpoints  
      3. Building of the UI  
4. Testing  
   1. Unit  
      1. Writing of the unit tests.  
      2. Integration of the unit tests into dev pipelines.  
   2. Integration  
      1. Writing of the integration tests.  
      2. Validation of the system cohesion and interconnection.  
   3. Regression  
      1. Full-fledged testing of the functionality with the user in the loop.  
      2. Identification of underdeveloped features based on users’ feedback.  
5. Deployment  
   1. GitOps infrastructure  
      1. Creation of the necessary repositories and organisational spaces.  
      2. Creation of common deployment resources in Kubernetes.  
      3. Support of the canary deployment feature.  
   2. Development stage  
      1. Set up of the development servers with a shared network.  
      2. Spin up of the VMs and data storage.  
   3. Staging environment  
      1. Set up the staging environment.  
      2. Set up of the secure RBAC.  
      3. Set up of the quality gates.  
   4. Production stage  
      1. Set up the production instances in a multi-cloud environment.  
      2. Connection of the metrics, logging, and analytics services.  
6. Project Management  
   1. Resources  
      1. Assessment of the available resources and the budget.  
      2. Covering of the potential development overheads and risks.  
   2. Timeline  
      1. Assessment of the desired project time bounds.  
      2. Covering critical tasks with extra time.  
   3. Risks  
      1. Prioritization of activities based on risk implied.  
      2. Preparation of a risk mitigation plan.  
      3. Preparation of a monitoring and risk-coping strategy.