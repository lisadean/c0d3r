# C0D3R 

C0D3R is a web application that facilitates genuine and meaningful interactions between programmers in search of a like-minded partner.


## Team Members

* Joshua Owens

* Lisa Dean

* Josh Brown

## Tools we used

* Javascript
* node.js
* Express
* Express-handlebars
* PostgreSQL
* Bulma
* OAuth (GitHub)

## Where to find it

Our project in it's entirety is hosted at http://www.trashpandapals.com to login and check it out.

Our git repository is at https://github.com/jko113/c0d3r to take a look at the code. 


## Walkthrough

[![Demo Video](readmeimg/youtubeimg.png)](http://www.youtube.com/watch?v=xxSGhGFpLyM "C0D3R ")

### Login Page

* The first page the user sees when they visit our site

<img src="readmeimg/loginpage.png">


### Home Page

* After logging in the user will be taken to the home page. Here you are shown a handful of profile cards and
can navigate to other sections of the site from the header at the top of the page.

<img src="readmeimg/homepage.png">

### Profile

* Here the user can view a detailed profile page. If the user is the owner of the page they will be given the option to 
edit the information on their profile or delete their account from the site altogether

<img src="readmeimg/profilepage.png">


### Search

* Here the user can choose from a variety of fields to narrow down the profiles their shown. The search works by or/and.  

<img src="readmeimg/searchpage.png">

### Messages

* Acts as an in app mailbox. A user can get display their message or compose a new message to another user by their GitHub alias.

<img src="readmeimg/messagespage.png">

### Logout

* Ends the users current session in the app and return to the login page.


## Things we're proud of

#### SQL generation for AND/OR searches and dummy data 
* Search implemented by passing in a JSON object from the search form which triggers dynamic SQL generation based on a dictionary of possible statement permutations. Dummy data insert statement generated via JS program that randomly generates most fields in the users table, including date-time values


#### Get random users feature
* Ensures the current user is not included in the result set. Draws from the JS Math library’s random() function. Keeps track of users being displayed to prevent duplicates

~~~
function getRandomUsers(current_user_id, num_users) {   return getAllUserIds()
    .then((arrayOfAllUserIds) => {       if (num_users > arrayOfAllUserIds.length) {
        throw new Error('number of users specified must be <= the dataset’);
      }       let randomIds = [];
      for (let i = 0; i < num_users; i++) {
        let randomIndex = Math.floor(Math.random() * intIds.length);
        let randomValue = intIds[randomIndex];
        randomIds.push(randomValue);
        intIds.splice(randomIndex, 1);
      }
    let initial = 'SELECT * FROM users WHERE user_id != $1 AND user_id IN (‘; let userIds = ’’; let final = ');’;
     randomIds.forEach((item, index) => {
      index === randomIds.length - 1 ? userIds += item: userIds += item + ', ‘; 
    });
     results = initial + userIds + final;
    return db.any(results, [current_user_id]);
  })
~~~

## Future Additions

#### New button to delete profile
~~~
app.post('/delete', (req, res) => {
    db.getUserByGithubId(req.session.passport.user.id)
        .then((data) => {
            db.deleteUser(data.user_id)
                .then((deleteData) => {
                    req.session.destroy();
                    res.redirect('/');
                })
                .catch(console.log)
        })
        .catch(console.log)
});
~~~
##### Current implementation requires that we make changes to the schema and at the point at which we looked at deletion we were 
##### hesitant to make those changes.

~~~
CREATE TABLE message_recipients (
    message_id integer REFERENCES messages (message_id) ON DELETE CASCADE,
    recipient_id integer REFERENCES users (user_id) ON DELETE CASCADE,
    is_read boolean,
    PRIMARY KEY (message_id, recipient_id)
);
~~~

##### By adjusting this function to remove references from other tables we could implement it without making changes to the schema

~~~
function deleteUser(id) {
    return db.result('DELETE from users WHERE user_id = $1', [id]);
}
~~~


#### Infinite scrolling on home page

##### This is the only snippet of our app that utilizes jQuery and rather than linking the library we opted to use a button for pagination until this snippet could be rewritten in vanilla JS
~~~
function scrollPage(){
    $(window).scroll(function() {
        setTimeout(scrollTimeout, 800);
    });
};

function scrollTimeout() {
    let hiddenCards = document.querySelectorAll('.hide');
    let hiddenArr = Array.from(hiddenCards);
    if($(window).scrollTop() + $(window).height() == $(document).height()) {
        for (i=0; i<5; i++){
            if (hiddenArr[0]){
                hiddenArr[0].classList.remove('hide');
                hiddenArr = hiddenArr.slice(1, hiddenArr.length);
            } else {
                let refreshButton = document.querySelector('.refresh');
                refreshButton.classList.remove('hide');
            };
        };
    };
};
~~~

#### Mobile horizontal swiping on the home page

* A similar feature to vertical pagination we woulld like to implement a horizontal swipe feature on the home page in future iterations.

#### Utilize SASS features on Bulma

* The styling library we used enables you to use SASS functionality which we didnt take advantage of in this project, but would like to revisit in future iterations.

#### Streamline/strengthen/secure search feature

#### Introduce message threading

#### Block user
