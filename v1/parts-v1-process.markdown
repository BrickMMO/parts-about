<style>@import url("//readme.codeadam.ca/readme.css");</style>

## Process

This applciation directory will include information on the following:

1. A list of LEGO® themes.
2. Information on a theme and a list of sets within a chosen theme.
3. Information on a set and parts and minifigures inlcuded in the chosen set.
4. Information on individual parts.
5. Information on individual minifigures.

There is no backend for this project. Currenly all backend functionality in completed by importing the raw [Rebrickable](https://rebrickable.com/downloads/) SQL files. In the future the import can be automated and run once a month using [CRON](https://en.wikipedia.org/wiki/Cron).

*** 

### Completed by Instructor

The [parts-v1](https://github.com/BrickMMO/parts-v1) includes a basic PHP core also used in [flow-v1](https://github.com/BrickMMO/flow-v2) and [stop-start-continue-v1](https://github.com/BrickMMO/stop-start-continue-v1).

There are twelve tables for this project provided by [Rebrickable](https://rebrickable.com/downloads/):

- colors
- elements
- inventories
- inventory_minifigs
- inventory_parts
- inventory_sets
- minifigs
- parts
- part_categories
- part_relationships
- sets
- themes

The database structure is as follows:

![BrickMMO Parts Schemea](/images/v1-schema.png)







There is an import script that imports a list of colours from the [Rebrickable API](https://rebrickable.com/api/v3/docs/) into the `colours` and `externals` tables. This file is located at `/import.php`.

There is a home page named `index.php` that displays a list of the colours from the `colours` table.

*** 

### Steps

1. **Design**

    Using [Figma](https://www.figma.com/) (or your design tool of choice) mock up the application. The application should include three pages:

    1. **Home Page**

        A list of colours in a well presented format. Each colour could include the colour name, the RGB value, and/or a sample. Each colour should link to a profile page.

    2. **Profile Page**

        A page with all the details about a single colour. This could include the colour name, the RGB value, a sample of the colour, a sample Brick image, alternative names, and links to alternative sources (like [BrickLink](https://bricklink.com/), [BrickOwl](https://bricklink.com/), etc...). 

    3. **Conversion Page**

        A page that allows the visitor to enter a colour (name, RGB, or CMYK), and the form will display the closet colour(s) to the provided colour. The results should be displayed in a nice visual format.

    The design style should mimic that of the [Passive Aggressive Password Machine](https://trypap.com/):

        - Nice large fonts
        - Simple layout
        - Well spaced out
        - Simple forms

    This project will use [Bootstrap](https://getbootstrap.com/). Review the Bootstrap website and avalable components and incorporate these into your design. 

    When complete, present your design to your instructor before moving on to the next step. 

2. **Local Server**

    Install a tool such as [MAMP](https://www.mamp.info/) on your computer. Open the MAMP progam and start the server. 

    Open the MAMP home page. It should be something like [http://localhost:8888](http://localhost:8888) on a Mac or [http://localhost](http://localhost] on a PC.

    In another tab open up [phpMyAdmin](https://www.phpmyadmin.net/). If you're using MAMP it will be something like [http://localhost:8888/phpMyAdmin/](http://localhost:8888/phpMyAdmin/) on a MAC or [http://localhost/phpMyAdmin/](http://localhost/phpMyAdmin/) on a PC.
    
3. **Database**

    Using phpMyAdmin create a new database named `brickmmo_colours` and import the two SQL files in the `/imports` folder. You should now have two empty tables named `colours` and `externals`.

4. **Source Code**

    Clone the [colours-v2](https://github.com/BrickMMO/colours-v2) repo into your MAMP root folder (or point MAMP to your cloned directory). Restart the server if needed. 

    Open up the `/env` file and change the database variables to `brickmmo_colours` and the MAMP defaults:

    ```php
    DB_HOST=localhost
    DB_DATABASE=brickmmo_colours
    DB_USERNAME=root
    DB_PASSWORD=root
    ```

    At this point you can open open the BrickMMO Colours home page. It will be empty, but there should be no PHP errors. 

5. **Rebrickable API**

    Register for a [Rebrickable API](https://rebrickable.com/api/) key. Put the key ino the `.env` file:

    ```php
    REBRICKABLE_API_KEY=12345678901234567890123456789012
    ```

    Run the `import.php` script. This can be done by changing the URL to [http://localhost:8888/import.php](http://localhost:8888/import.php) on a Mac or [http://localhost/import.php](http://localhost/import.php] on a PC.

    Open up phpMyAdmin and your tables should now be full of records.

> [!TIP]  
> Steps six through twelve have a project task in GitHub. Make sure to assign these tasks when starting. And then use these tasks to create your branch and a pull request when done.

6. **Home Page**
    
    Create the home page. Review the [Rebrickable API](https://rebrickable.com/api/v3/docs/) and the `colours` and `externals` tables to see what data is avaiable to put on the home page. Make these changes to the `/index.php` file. Don't worry too much about the design yet. 

> [!NOTE]  
> At this point do not worry about the design. Just use basic HTML, black text, and Times New Roman. No CSS needed. 

7. **Profile Page**

    On the home page you will link each colour to `\profile.php?id=1`. The number one will be replaced with the `id` value form the colours table. It will look something like this:

    ```php
    <?php while($colour = mysqli_fetch_assoc($result)): ?>

        <a href="profile.php?id=<?=$colour['id']?>"><?=$colour['name']?></a>
        
    <?php endwhile; ?>
    ```

    Create the a page named `profile.php`. Write the PHP and HTML to display a basic colour profile. Use the code in `/index.php` as a starting point. 

    Add some of the following data:

        - Colour Name
        - RGB value
        - Sample of the colour (there is an example of this in `/index.php`)
        - Links to alternative sources (like [BrickLink](https://bricklink.com/), [BrickOwl](https://bricklink.com/), etc...)

    You will need to write two `SELECT` queries to fetch as much information about the selected colour from the database. They will need to incorporate the `$_GET['id']` variable into the query. 

8. **Sample Brick Image**

    You can add a sample brick by using the [Rebrickable]() media. For example, the following URL:
    
    ```
    https://cdn.rebrickable.com/media/thumbs/parts/ldraw/4/3003.png/85x85p.png
    ```

    Will display the following image:

    ![Red 3001 Brick](https://cdn.rebrickable.com/media/thumbs/parts/ldraw/4/3003.png/85x85p.png)

    The image URL consists of the following parts:

        - Rebrickable media URL: `https://cdn.rebrickable.com/media/thumbs/parts/ldraw/`
        - The colour ID using the `rebrickable_id` value from the `colours` table: `<?=$colour['rebrickable_id']?>`
        - The LEGO® brick ID using, you can look these up at the [LEGO® Pick-a-Brick Store](https://www.lego.com/en-ca/pick-and-build/pick-a-brick)
        - And the width and height of the image

9. **Conversion API**

    Create a file named `/api.php`. For now you can test this file by visiting [http://localhost:8888/api.php](http://localhost:8888/api.php) on a Mac or [http://localhost/api.php](http://localhost/api.php] on a PC. At this point it will be an empty page.

    Copy the `include` statements from one of your other PHP files. 

    ```php
    <?php

    include('includes/connect.php');
    include('includes/config.php');
    include('includes/functions.php');
    ```

    Add `?colour=336699` to your URL. Just to test the colour URL variable, in the `\api.php` file, output the colour provided in the URL. You will need to use `$_GET`.

    Add code that will convert the colour from the URL variableto the closest colour from the `colours` table. I would recommend incuding the three closest colours. **This is the hardest part!**

    Output the three colours using JSON.

10. **Conversion Tool**

    The last page needs to use the `/api.php` file to convert a provided colour to the closet LEGO® pallette colour. This process will follow these steps:

    1. The visitor will enter a colour name, RGB, or CMYK value into the form.
    2. The visitor will click submit.
    3. JavaScript will make an API call to `\api.php?colour=336699`. This API call will return a JSON list of the closest colours.
    4. JavaScript will then read the result and display the results. 

    Create a page named `/convert.php`. Add a link on the home page to this new page. 

    Add the form to this page. 

    Add JavaScript to validate the colour entered into the form. On submit make an API call to `/api.php`. PParse the JSON response and display the top three colours. 

11. **Apply Design**

    Apply the designs from stop one!

12. **About Colours** 

    Update the [colours-about](https://github.com/BrickMMO/colours-about) Markdown! Add your names to the `v2.markdown` page. 

[&#10132; Back to V2](/colours-about/v2

---

<a href="https://brickmmo.com">
<img src="https://brickmmo.com/images/brickmmo-logo-horizontal.jpg" width="100">
</a>
