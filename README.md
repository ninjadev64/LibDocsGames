## LibDocsGames
A simple wrapper for Apps Script to make games in Google Docs!

### Getting started
1. Create a Google Doc, and make a new table. This will be your game tilemap. 
2. Choose a background colour. Then, open Table Properties > Colour and change the table border and cell background colour to your background colour.
    1. You may also change the border width to the minimum (1px).
3. Open a new Apps Script for that document.
4. Include the library in your project with the script ID `1ygkKDEaJU9XMspL1YIkzxbqsWYQtQ9DawlfTqfEcJlKm4UEOBeVR2lGL`. Select the latest version.

### Basic demo
In this game, you, the frog, can move left and right. If you touch the duck, the game ends.
```javascript
function Images() {
  this.duck = DriveApp.getFileById("105B6R09gA3UgLyv5ZjnaKtKRJpS_guXI").getAs("image/png").setName("Duck");
  this.player = DriveApp.getFileById("1-oQ4TZ0YAGU7BoXtogvS9_J2k2FVfAGl").getAs("image/png").setName("Frog");
  this.grass = DriveApp.getFileById("1LdXVQYdmmtJb4TYDSjGXlgcK1buJL3Yq").getAs("image/png").setName("Grass");
}
const images = new Images();

function onOpen(e) {
  // Create a menu item in the Extensions toolbar labelled Play, which will call the frogHop(); function when clicked.
  DocumentApp.getUi().createAddonMenu().addItem("Play", "frogHop").addToUi();
}

function frogHop() {
  LDG.init(0, "#34c6eb"); // The index of the game table in all of the tables in the document, followed by the background colour in hex.

  let grass = images.grass;
  LDG.tile(grass, 0, 7); LDG.tile(grass, 1, 7); LDG.tile(grass, 2, 7); LDG.tile(grass, 3, 7); // Tiles are never redrawn
  LDG.tile(grass, 4, 7); LDG.tile(grass, 5, 7); LDG.tile(grass, 6, 7); LDG.tile(grass, 7, 7);

  var player = new LDG.Sprite("Player", images.player, 4, 6); // Label, image, x, y
  var enemy = new LDG.Sprite("Enemy", images.duck, 2, 6);

  gameloop: while (true) {
    let moveKeyCell = DocumentApp.getActiveDocument().getBody().getTables()[1].getRow(0).getCell(0); // A one-cell table for the user to provide input in
    switch (moveKeyCell.getText().charAt(0)) { // Execute one input character "keypress" per frame
      case "a": player.position.x -= 1; break;
      case "d": player.position.x += 1; break;
    }
    moveKeyCell.setText(moveKeyCell.getText().substring(1)); // Remove the input character that was just executed

    if (player.position.x == enemy.position.x && player.position.y == enemy.position.y) {
      console.log("Game over!");
      break;
    }

    LDG.draw(); // Redraw sprites and carry out other basic game logic
  }
}
```

### FAQ

**Q.** Why is it so slow?

**A.** When an image is redrawn, the browser has to reload it. A key way to fix this is use tiles for static game objects, which are never redrawn.



**Q.** Can I see the source?

**A.** Sure! The library is public, and you can use the script ID to find it.



**Q.** Help!

**A.** Join my Discord! [https://discord.gg/jyJJWjqbFP](CitrusDev)
