# Bake Off Challenge

Today, we'll be making an app to create delicious baked goods and allow the judges to rate our creations!

## Deliverables

1. **When the page first loads**, all the bakes should display in the sidebar.
2. **When a bake is clicked in the sidebar**, the details for the bake should show up in the detail area.
3. When the "Make New Bake" button is clicked, a popup (modal) will display a form (this part is already done for you). **When this form is submitted**, a new bake should be created in the backend and added to the list of bakes displayed in the sidebar.

## Bonus Deliverables
- **In the detail view, when a user enters a rating and submits**, the rating should be saved in the backend and persisted in the frontend.
- **When the user clicks 'Judge Bakes'**, the winner should be revealed in the sidebar.

## API Setup

From the `bake-off-challenge-backend` directory, run the following commands to set up and start the server:

```sh
$ bundle install            # Install gems
$ rails db:migrate db:seed  # Migrate and seed
$ rails s                   # Start the server
```

A full list of available routes can be found by visiting `http://localhost:3000/rails/info/routes`.

### Get All Bakes (Deliverable 1)

**When the page first loads**, all the bakes should display in the sidebar.

To create each list item in the `#bakes-container` in the sidebar, use the following HTML as a template:

```html
<li class="item" data-id="1">Rahul’s Chocolate-dipped Orange Madeleines</li>
```

To get the data from the server, you will need to make a `fetch` request:

*Get All Bakes*
```
Route: GET /bakes

Example response:
[
  {
    "id": 1,
    "name": "Rahul’s Chocolate-dipped Orange Madeleines",
    "description": "These dainty treats are filled with homemade orange curd. You’ll make more curd than you need, so keep the remainder to have on toast another time.",
    "image_url": "https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/05/Choc_Orange_Madalienes_1024x450-1024x450.png",
    "rating": 0
  },
  {
    "id": 2,
    "name": "Alice’s Orange & Cardamom ‘Ice Cream’ Buns",
    "description": "Fragrant breads baked to resemble ice-cream tubs are topped with a delicious cream-cheese icing and sprinkles. They are great fun to serve to children.",
    "image_url": "https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/10/icecreams_forweb.jpg",
    "rating": 2
  }
]
```

### Show Bake Details (Deliverable 2)

**When a bake is clicked in the sidebar**, the details for the bake should show up in the detail area.

The HTML of how you should render the selected bake in the `#detail` div should look like this:

```html
  <img src="https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/10/icecreams_forweb.jpg" alt="Alice’s Orange & Cardamom ‘Ice Cream’ Buns">
  <h1>Alice’s Orange & Cardamom ‘Ice Cream’ Buns</h1>
  <p class="description">
    Fragrant breads baked to resemble ice-cream tubs are topped with a delicious cream-cheese icing and sprinkles. They are great fun to serve to children.
  </p>
  <form id="score-form">
    <input type="number" name="score" min="0" max="10" step="1" value="10">
    <input type="submit" value="Rate">
  </form>
```

You can either use the data from your initial fetch with ALL bakes to render the detail view, or you can make another fetch to the following endpoint:

*Get One Bake*
```
Route: GET /bakes/:id

Example Response:
{
  "id": 1,
  "name": "Rahul’s Chocolate-dipped Orange Madeleines",
  "description": "These dainty treats are filled with homemade orange curd. You’ll make more curd than you need, so keep the remainder to have on toast another time.",
  "image_url": "https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/05/Choc_Orange_Madalienes_1024x450-1024x450.png",
  "rating": 0
}
```

### Create New Bake (Deliverable 3)

When the "Make New Bake" button is clicked, a popup (modal) will display a form (this part is already done for you). **When this form is submitted**, a new bake should be created in the backend and added to the list of bakes displayed in the sidebar.

To get your new bake to persist in the backend, you'll need to make the following request:

*Create Bake*
```
Route: POST /bakes

Required Headers: {
  "Content-Type": "application/json"
}

Required Keys In Body: {
  name: "string",
  image_url: "string",
  description: "string"
}

Example Response:
{
  "id": 1,
  "name": "Rahul’s Chocolate-dipped Orange Madeleines",
  "description": "These dainty treats are filled with homemade orange curd. You’ll make more curd than you need, so keep the remainder to have on toast another time.",
  "image_url": "https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/05/Choc_Orange_Madalienes_1024x450-1024x450.png",
  "rating": 0
}
```

### Rate Bake (Deliverable 4)

**In the detail view, when a user enters a rating and submits**, the rating should be saved in the backend and persisted in the frontend.

To get your rating to persist in the backend, you'll need to make the following request. **NOTE**: take special notice of the required headers! Only judges can submit a rating, so you'll have to submit an additional header to authorize this request.

*Rate Bake*
```
Route: POST /bakes/:id/ratings

Required Headers: {
  "Content-Type": "application/json",
  "Authorization": "Bearer 699a9ff1-88ca-4d77-a26e-e4bc31cfc261"
}

Required Keys In Body: {
  score: number
}

Example Response:
{
  "id": 1,
  "name": "Rahul’s Chocolate-dipped Orange Madeleines",
  "description": "These dainty treats are filled with homemade orange curd. You’ll make more curd than you need, so keep the remainder to have on toast another time.",
  "image_url": "https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/05/Choc_Orange_Madalienes_1024x450-1024x450.png",
  "rating": 10
}
```

### View Winner (Deliverable 5)

**When the user clicks 'Judge Bakes'**, the winner should be revealed in the sidebar.

After rating your bakes, you can make a fetch formatted using the instructions below to find the winner. In order to mark the winner in the DOM, add a class of `winner` to the list item in the `#bakes-container` for the winning entry.

*Get Winning Bake*
```
Route: GET /bakes/winner

Example Response:
{
  "id": 1,
  "name": "Rahul’s Chocolate-dipped Orange Madeleines",
  "description": "These dainty treats are filled with homemade orange curd. You’ll make more curd than you need, so keep the remainder to have on toast another time.",
  "image_url": "https://thegreatbritishbakeoff.co.uk/wp-content/uploads/2019/05/Choc_Orange_Madalienes_1024x450-1024x450.png",
  "rating": 0
}
```