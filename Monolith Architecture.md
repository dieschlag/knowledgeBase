Architecture where all components (the interface and business logic) are  tightly integrated and deployed together.

As a consequence:
- the code is hard to maintain, any change requires the redeployment of the entire app
- the database is hard to maintain. Everything must be stored in a single database.
- Deploying an update is hard, since an error somewhere can affect the entire app
- Scaling is difficult, and might require the deployment of several instances of the monolith even if only one component needs extra ressources
