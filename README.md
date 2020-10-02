# Propser-IT-Internship-Projects

# Python Live Project

#### Within a two-week Sprint, I built a Django-based web application alongside a team of developers, with each of us working on our own specific Hobby Manager apps which could be navigated to from one shared home page. Honing my front-end and back-end development skills, I developed an Anime-themed Hobby Manager App, featuring a database that allows users to keep track of info about their favorite characters. Below I have reproduced code snippets and screenshots of the webpages.

### View Homepage Function

```python
# View function that renders homepage
def home(request):
    return render(request, 'Anime/anime_home.html')
```
![Screenshot](https://raw.githubusercontent.com/sparr92/sparr92.github.io/master/images/Anime_Index.JPG)

### View Character Index Function

```python
#View function that controls the main index page - list of characters
def character_index(request):
    get_characters = Character.Characters.all()      #Gets all the current characters from the database
    context = {'characters': get_characters}      #Creates a dictionary object of all the characters for the template
    return render(request, 'Anime/anime_index.html', context)
```
![Screenshot](https://raw.githubusercontent.com/sparr92/sparr92.github.io/master/images/Character_Index.JPG)

### Front-End Styling for Character Index Page

```html
<section >
    <div class="flex-container" id="charindex">
        <table class="table-striped">
            <tr>
                <th class="col-md">Name</th>
                <th class="col-md">Age</th>
                <th class="col-md">Birthday</th>
                <th class="col-md">Hair Color</th>
                <th class="col-md">Zodiac Sign</th>
                <th class="col-md">Likes</th>
                <th class="col-md">Dislikes</th>
                <th class="col-md">User Rating</th>
                <th class="col-md">Notes</th>
            </tr>
            {% for character in characters %}     <!-- creates a new row for each character in the collection -->
                <tr>
                    <td class="col-md">{{character.char_name}}</td>
                    <td class="col-md">{{character.char_age}}</td>
                    <td class="col-md">{{character.char_DoB}}</td>
                    <td class="col-md">{{character.hair_color}}</td>
                    <td class="col-md">{{character.zodiac_sign}}</td>
                    <td class="col-md">{{character.likes}}</td>
                    <td class="col-md">{{character.dislikes}}</td>
                    <td class="col-md">{{character.user_rating}}</td>
                    <td class="col-md">{{character.notes}}</td>
                    <td class="col-md"><a href="{{character.pk}}/Details"><button class="primary-light-button" id="info-button">Details</button></a></td>    <!-- creates a link for that specific character -->
                </tr>
            {% endfor %}
        </table>
        <button class="primary-bright-button" type="button" onclick=" location.href='{% url 'add_character' %}'">Add a character</button>
    </div>
</section>
```

### Add Character to Database Function

```python
# View function to add a new character to the database
def add_character(request):
    form = CharacterForm(request.POST or None)     #Gets the posted form, if one exists
    if form.is_valid():                         #Checks the form for errors, to make sure it's filled in
        form.save()                             #Saves the valid form/character to the database
        return redirect('character_index')            #Redirects to the index page, which is named 'characters' in the urls
    else:
        print(form.errors)                      #Prints any errors for the posted form to the terminal
        form = CharacterForm()                       #Creates a new blank form
    return render(request, 'Anime/anime_create.html', {'form': form})
```
![Screenshot](https://raw.githubusercontent.com/sparr92/sparr92.github.io/master/images/Add_Character.JPG)

### View Character Details Function

```python
#View function to look up the details of a character
def character_details(request, pk):
    pk = int(pk)                                #Casts value of pk to an int so it's in the proper form
    character = get_object_or_404(Character, pk=pk)   #Gets single instance of the character from the database
    context = {'character': character}                   #Creates dictionary object to pass the character object
    return render(request, 'Anime/anime_details.html', context)
```
![Screenshot](https://raw.githubusercontent.com/sparr92/sparr92.github.io/master/images/Character_Details.JPG)

### Front-End Styling for Character Details Page

```html
<section>
    <div class="flex-container footy" id="chardetails">
        <table>
            <tr>
                <th>Name:</th>
                <td>{{character.char_name}}</td>
            </tr>
            <tr>
                <th>Age:</th>
                <td>{{character.char_age}}</td>
            </tr>
            <tr>
                <th>Birthday:</th>
                <td>{{character.char_DoB}}</td>
            </tr>
            <tr>
                <th>Hair Color:</th>
                <td>{{character.hair_color}}</td>
            </tr>
            <tr>
                <th>Zodiac Sign:</th>
                <td>{{character.zodiac_sign}}</td>
            </tr>
            <tr>
                <th>Likes:</th>
                <td>{{character.likes}}</td>
            </tr>
            <tr>
                <th>Dislikes:</th>
                <td>{{character.dislikes}}</td>
            </tr>
            <tr>
                <th>User Rating:</th>
                <td>{{character.user_rating}}</td>
            </tr>
            <tr>
                <th>Notes:</th>
                <td>{{character.notes}}</td>
            </tr>
        </table>

        <hr />
        <button class="primary-bright-button" type="button" onclick=" location.href='{% url 'character_edit' character.pk %}'">Edit
        </button>
        <button class="primary-bright-button" type="button" onclick="location.href='{% url 'character_delete' character.pk %}'">Delete
        </button>
        <button class="primary-bright-button" type="button" onclick=" location.href='{% url 'character_index' %}'">Back to Character Index</button>
    </div>
</section>
```

### Edit Character Details Function

```python
# View function to edit details of a character
def character_edit(request, pk):
    character = get_object_or_404(Character, pk=pk)  # get character record if it exists
    if request.method == "POST":
        form = CharacterForm(request.POST, instance=character)  # gets the posted form
        if form.is_valid():  # Checks the form for errors, to make sure it's filled in
            character = form.save()  # Saves the valid form/character to the database
            character.save()
            return redirect('character_details', pk=character.pk)
    else:
        form = CharacterForm(instance=character)
    return render(request, 'Anime/anime_edit.html', {'form': form})
```
![Screenshot](https://raw.githubusercontent.com/sparr92/sparr92.github.io/master/images/Character_Edit.JPG)

### Delete Character Function

```python
# View function to delete a character
def character_delete(request, pk):
    character = get_object_or_404(Character, pk=pk)
    if request.method == "POST":
        character.delete()
        return redirect('character_index')  # redirects to the index page
    return render(request, "Anime/anime_delete.html", context={'character': character})
```
![Screenshot](https://raw.githubusercontent.com/sparr92/sparr92.github.io/master/images/Character_Delete.JPG)

### Skills

- Worked on a Sprint with other developers within an Agile framework, helping others find fixes/debug, and reaching out for help in addition to solving problems on my own
- Gained real experience working within a larger project in a team environment, being communicative and collaborative
- Practiced researching new methodologies and frameworks to put to use in my projects
- Used Django, Python, PyCharm, Git, Azure, Bootstrap, Javascript, etc.


# C# Live Project

#### Within a two-week Sprint, I worked alongside a team of developers on an existing ASP.NET MVC 5 code-first application, correcting known errors on the website of a local non-profit organization. Assigned the fix of restricting visibility and access to an Edit button on a particular page, I researched how to determine the user role of the logged-in user, and restricted access both on the front-end and back-end. Below I have reproduced code snippets.

### Hiding the Edit Button from Non-Admin Users on the Front-End

```C#
<p>
    @if (Request.IsAuthenticated && User.IsInRole("Admin"))
    {
        @Html.ActionLink("Edit", "Edit", new { id = Model.PartID }) @: |
    }
    @Html.ActionLink("Back to List", "Index")
</p>

```
### Limited Access to Edit Function to Admin Only on the Back-End

```C#
        [Authorize(Roles = "Admin")]
        // GET: Part/Edit/5
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                 return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Part part = db.Parts.Find(id);
            if (part == null)
            {
                return HttpNotFound();
            }
            Part person = db.Parts.Find(id);
            if (person == null)
            {
                return HttpNotFound();
            }
            Part production = db.Parts.Find(id);
            if (production == null)
            {
                return HttpNotFound();
            }

			ViewData["Productions"] = new SelectList(db.Productions, "ProductionId", "Title", part.Production.ProductionId);
			
			ViewData["CastMembers"] = new SelectList(db.CastMembers, "CastMemberId", "Name", part.Person.CastMemberID);

            return View(part);
        }
```
