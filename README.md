# Propser-IT-Internship-Projects

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
