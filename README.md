![ga_cog_large_red_rgb](https://cloud.githubusercontent.com/assets/40461/8183776/469f976e-1432-11e5-8199-6ac91363302b.png)

# Read Me – Project 3 – Game of Thrones Full-Stack app

The third project in the General Assembly Software Engineering Immersive was our first full-stack group project. The project involved collaboration within a team of three—myself and two other students—and required the development of a full-stack MERN (MongoDB, Express.js, React.js, Node.js) application. This project served as a culmination of the technologies we had extensively learned over the preceding month. 
 
**Deployment link **

Deployment link:** **[https://gameofthronesfullstack-b965d7354f59.herokuapp.com/](https://gameofthronesfullstack-b965d7354f59.herokuapp.com/)

**Brief**

 We received a set of project requirements that can be succinctly summarised as follows:



* Full stack app, using an Express API.
* Consume the API with a separate front-end built by React.
* A full product with CRUD functionality on at least one model.
* And to be deployed online where it is accessible to everyone.

**Planning**

Amongst those technical requirements, there were also some other requirements which had to be fulfilled prior to starting the project, these included a wireframe and a written project plan which were looked over by the tutor where we were expected to explain in detail the features of the app, how we planned on implementing them and how they were going to work, the main bases of this signing off process to make sure we had a good understanding of what our Minimum Viable Product was going to be. Stretch goals were also discussed during these signing off presentation. \


<img width="678" alt="Screenshot 2024-02-13 at 14 00 26" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/a9cd763d-95df-4c0e-8d15-fcb8599613ea">


The wireframe above depicts the earliest conception of our project which changed as we went through the production process. The concept for our project was a fan tribute site to game of thrones, we wanted a webpage that could display the famous places, characters, and the famous houses (as in royal houses) within the vast lore of the world of Game of Thrones. Users of the webpage would be able to create their own characters that could be added to the character list and be incorporated into the Royal houses by allowing that user to pick a house. The planning was done using a mind map, where ideas were first discussed and then we were able to create a wireframe depicting what we would like the webpage to look like, the use of the mind map meant that we were also able to write down ideas and their descriptions and how they could potentially be implemented. Planning was done over two days, with wireframing being mind mapping and wireframing being completed concurrently. We also created a document which contained the models and all the relationships between them.

A part of the plan was also the division of labour, as this would have been our first time doing a project with a backend, we all wanted to remain involved with the creation of the back-end of the project, within the backend I was tasked with the creation  of the character model, along with the controllers which would give us the CRUD capability on the webpage, I also played a major hand within anything that had anything to do with authentication on the back-end, other tasks were done by my teammates and other tasks were done together whilst in production. I will start by going through any back-end features I created or any that I played a major hand in. A Trello board was set up by my colleague where we able to set up tasks and assign them, which made it possible to be able to see where we were at in terms of our production:


<img width="603" alt="Screenshot 2024-02-13 at 14 02 18" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/014923c5-b664-49f6-8bbc-1a2cc1b1b832">



**Technology**

**Back End:**



* MongoDB (Database)
* Express.js (Web application framework)
* Node.js (JavaScript runtime environment)

**Front End:**



* React.js (JavaScript library for building user interfaces)
* HTML (Markup language for creating web pages)
* CSS (Styling language for web pages)

**Development Tools:**



* npm (Node Package Manager)
* Git (Version control system)
* VSCode (Integrated Development Environment)
* ESLint (Code linter for identifying and fixing problems in the JavaScript code)

**Build**

**BACK-END** 
 
At the outset of this project, I prioritised meticulous planning to ensure a structured approach and comprehensive understanding before deep diving into coding. One of the primary steps was crafting a user model using the Mongoose library within Node.js. This model served as the foundational structure for the users' data. Below is the code snippet representing the user schema: 
 

<img width="375" alt="Screenshot 2024-02-13 at 14 05 33" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/8ef79a6c-3c81-4309-a68a-06956aef1d1b">



The user schema encompassed fields for **username**, **email**, **password**, and an optional **image**. Additionally, it included functionality for password hashing, validation of password confirmation, and the creation of virtual fields. The **userSchema()** was used to ensure virtual fields would be available on UserSchema, an important bit of code which meant that on the front-end the display of Characters that were created by the User would be viewable and usable with additional functionality potentially. 
 
The declaration of **userSchema.virtual('passwordConfirmation')** establishes a virtual field named **passwordConfirmation** within the user schema. This virtual field doesn't persist in the database but provides a means for developers to temporarily store and validate a user's entered password confirmation during certain operations. 


<img width="411" alt="Screenshot 2024-02-13 at 14 07 18" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/8e4c6d72-f665-461e-8eea-a5d8a1894e1c">


The **.set()** function attached to this virtual field assigns the provided value to **_passwordConfirmation** within the schema instance. As part of the user schema setup, I've included middleware functions utilising Mongoose. These functions, specifically **userSchema.pre('validate', ...)** and **userSchema.pre('save', ...)**, execute critical tasks just before the validation and saving processes occur for a user entity. These mechanisms allow us to perform crucial checks and alterations before the data undergoes validation and is eventually persisted to the database. Within the pre-validation middleware, I've implemented a conditional check to validate changes made to the password field. This check ensures that if the user modifies the password, it matches the **_passwordConfirmation**. If there's a discrepancy between the entered passwords, it halts the validation process and throws an error message prompting the user to ensure both passwords match for successful submission.

On the other hand, the pre-save middleware is instrumental in enhancing the security of user passwords. When a modification occurs in the password field, I've incorporated a mechanism that securely hashes the password. Utilising the bcrypt library, I've hashed the password with a salt generated through **bcrypt.genSaltSync(12)**. This hashing procedure significantly fortifies security by storing only the hashed password in the database, thus preventing sensitive information exposure. These middleware functions are integral components of the user schema, serving to validate password consistency and bolster overall security measures within the application. \
 
**User Controller Overview 
 
**Within my user controllers, I've encapsulated essential functionalities for user registration, login, profile retrieval, and profile image updates. These controllers handle interactions between the server and the user model, implementing necessary logic to ensure secure and efficient user operations. \
 

<img width="600" alt="Screenshot 2024-02-13 at 14 09 45" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/cb23fc51-ff9b-4109-ae11-05c238b56a1e">


The **register** controller function facilitates user registration by utilising the **User.create()** method from the database model. Upon receiving user details through the request body, this function creates a new user entry in the database. In case of successful registration, it returns a status code of 201 along with a welcoming message to the newly registered user.

**Authentication**


<img width="601" alt="Screenshot 2024-02-13 at 14 10 42" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/aa0de9ee-53b2-493c-aaf8-b9b85d31a51b">



For user authentication during login attempts, the **login** controller verifies the provided email and password. Using **User.findOne()** to search for the user's email in the database, it then compares the hashed password from the database with the plaintext password entered by the user. In the event of successful authentication, a JSON Web Token (JWT) is created using **jwt.sign()**, embedding the user's ID as the payload and encrypting it with a secret key. Upon successful validation, it returns a status code of 202 and issues a token, allowing access to authenticated routes.

Within the Back-end we also added a Secure Route function which would be used on top of our normal router to add a security, here is a picture of the code:  
 

<img width="600" alt="Screenshot 2024-02-13 at 14 14 34" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/1c6a87c8-fe48-44be-8a43-5f8240f0a044">



Implementing this secure route middleware significantly fortifies the site's security posture. By validating JWT tokens, verifying user existence, and controlling access to protected routes, I've bolstered the site's defences against unauthorised access attempts. This comprehensive authentication mechanism safeguards sensitive functionalities, ensuring that only authenticated users can access and interact with the protected endpoints, mitigating potential security vulnerabilities and reinforcing the overall integrity of the site.

**FRONT END**

The first job I took part in was creating the NavBar of the site, the component for the search bar itself can be found on **NavBarSearch.jsx.**, In building the **NavBarSearch** component, I focused on creating a user-friendly navigation feature that seamlessly integrates a search bar into the site's navigation. This component serves as an essential tool, allowing users to explore and discover diverse content across different sections of the site right from the navigation bar. A standout benefit of this component is its ability to enable early and effortless navigation within the site. The inclusion of the search bar at this stage provided us with immediate access to search functionality, allowing us to quickly find relevant content without the need to navigate extensively through the site's structure in the development process.

 The search bar with the User logged out and logged in.

One of the key highlights of this component lies in its utilization of asynchronous data fetching, powered by the **fetchData** function. This asynchronous approach enables efficient querying of the server for data related to houses, places, and characters in response to user input. The clever integration of the **await fetch** method ensures that the search functionality remains responsive and dynamic. The implementation intelligently filters fetched results based on the user's input. This means that as users type their queries, the component filters the data retrieved from different endpoints (**houses**, **places**, **characters**) to display only the most relevant content matching the search criteria. This smart filtering mechanism enhances the search experience by presenting users with tailored and pertinent results. 


 
**Authentication**

 
The **activeUser** function operates as a vital piece of the site's user authentication system. It functions by retrieving and assessing the authenticity of a stored token in the local storage. Upon invocation, it validates the token's existence and checks its expiry against the current time, ensuring its validity. This dynamic functionality plays a pivotal role in rendering different navigation elements based on the user's authentication status. When a user is authenticated, it triggers the display of specific navigation links, such as accessing the user's profile page, creating a character, and facilitating a smooth logout process. Conversely, for unauthenticated users, it renders links prompting them to register or login as shown in the most recent picture above. This conditional rendering of navigation elements within the JSX of the **Nav** component ensures a tailored and secure user experience, allowing users to seamlessly access functionalities based on their authentication status. \

<img width="202" alt="Screenshot 2024-02-13 at 14 18 01" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/589764cc-7f55-4b49-8015-b3225e4bb7f3">


Active user function. 
 
Amongst other responsibility one which took up a few days for me to work through was the authentication and whilst I also worked on the Profile Page and Registration page I thought I would summarise the work I did within the Login component and the functions used to facilitate the logging in process on the front-end and back-end. The login process begins on the front-end with the **Login** component. This component serves as the UI for users to enter their credentials. It consists of a basic form structure containing input fields for email and password. The component leverages React hooks like **useEffect** and React Router DOM hooks such as **useActionData** and **useNavigate**. \
 

<img width="604" alt="Screenshot 2024-02-13 at 14 18 58" src="https://github.com/NyashaDZT/GameOfThrones_fullstack/assets/124045473/47248ef9-81a1-4f1f-9b31-4e5377315cd5">


The login page.

Upon submission of the login form, the **loginUser** function in the front-end initiates an asynchronous HTTP POST request to the '/api/login' endpoint using Axios. It sends the user-entered credentials (email and password) as data in the request body. The **validateStatus: () => true** configuration is included to ensure handling of all response statuses. Meanwhile, the backend **/api/login** endpoint is handled by the **login** function. This function, upon receiving the request, checks if the user exists in the database based on the provided email. It then verifies if the passwords match using bcrypt. If the authentication is successful, it generates a JSON Web Token (JWT) using the **jwt.sign** method, assigning it an expiration time of 1 day. This token is then sent back to the front-end along with a success message via a JSON response with status code 202.

 \
Back on the front-end, the **Login** component utilises **useEffect** to monitor changes in the response received from the login request. If the response status is 202 (indicating a successful login), it saves the token using the **setToken** function, navigates the user to the homepage ('/') using **navigate** from **react-router-dom**, effectively redirecting the user after successful authentication. The styling for the **Login** component maintains simplicity, using basic HTML inputs and minimal CSS classes to ensure a clean and user-friendly interface.

**Challenges**

Throughout the project, I encountered common challenges in software development. One such hurdle was spending an extensive amount of time stuck on a particular issue, leading to time wastage and subsequent catch-up work. In hindsight, I realise that switching to a different task temporarily might have been a beneficial approach. This strategy could have prevented getting stuck for an extended period and potentially allowed for fresh insights when revisiting the problem later. Additionally, compiling and integrating extensive code segments, despite the project's relative ease (in the context of how difficult project one felt), proved challenging. Organising and aligning numerous code components demanded meticulous attention to detail to ensure seamless integration.

My reliance on documentation, particularly the React Bootstrap, Mongoose, and Express documentation, was pivotal. I invested significant time exploring these resources, highlighting their complexity and the need for a comprehensive understanding to effectively utilise their functionalities. Despite facing challenges in code integration and problem-solving, my dedication to navigating and leveraging documentation demonstrates resilience and a commitment to overcoming hurdles encountered throughout the project journey.

**Wins & Key Learnings**

Throughout our project journey, effective communication was the cornerstone of our success right from the project's inception. As a team, we prioritised communication, dedicating an initial three-hour Zoom session to meticulously plan our approach before seeking project approval. Leveraging tools like Trello for task management and zoom for regular meetings, including daily morning briefs and stand-ups, ensured our lines of communication remained crystal clear. Collaborating on Zoom provided us with a collaborative platform where ideas flowed freely, fostering a cohesive team environment despite remote working arrangements. Our strategic planning involved coordinating submission schedules into our development folder, aligning tasks around each other's progress. This approach enabled us to optimise our workflow, capitalise on collective insights, and ensure seamless integration of contributions, allowing us to harness the full potential of our diverse skill sets towards the project's success.

Our collaboration extended seamlessly onto GitHub, where I actively engaged in utilising version control practices such as cloning and fetching to ensure synchronisation within our team. We established a systematic approach to update each other upon pushing changes to our shared development folder, maintaining transparency, and facilitating a coherent workflow. Troubleshooting GitHub-related issues collectively became a norm, fostering a supportive environment where we efficiently addressed challenges as a team. Additionally, our commitment to open communication channels was instrumental, enabling continuous support even during non-synchronous work hours. This approach allowed for smooth knowledge sharing and facilitated mutual understanding, ensuring that all team members remained aligned and well-versed in the project's codebase

**Bugs **

There are no bugs to report from the project.

**Future Improvements**

The biggest future improvements I can see for the project would be to improve the overall styling of the profile project, this part of the project was done very quickly and this can be noted in its rather rough state that it was left in. Although it does display the necessary information, it does not display it in the most visually pleasing way. Another future improvement would be to allow users to upload their profile pictures directly on the register form. To begin with we did not feel as though we were going to have a profile picture and by the time it was added in as a feature we could only put the option of uploading it, directly on the profile page itself.
