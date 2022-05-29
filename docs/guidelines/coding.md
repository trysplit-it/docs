---
layout: default
title: Code style
parent: Guidelines
nav_order: 2
permalink: /guidelines/code
---

# Code Style Guidelines

## Indentation / Brackets

- All code or structures which are naturally contained within another should be indented with a tab.

    ```
    <Grid container>
        <Grid item>
            <Box/>
        </Grid>
    </Grid>
    ```

- Follow “The One True Brace Style”

    ```
    function foo() {
        console.log(“Hello world!”);
    }
    ```

## Naming Conventions

- Use lowerCamelCase for variables and UpperCamelCase for class names.

- Use CONSTANT_CASE for constants and environment variables.

## Organization

- All files in a folder should be related or have a similar purpose.

- File names should not repeat information provided by folder name.

```
/mobile
| –> Calendar.js (not MobileCalendar.js)
| –> Filter.js (not MobileFilter.js)
```

## Spacing

- Add spacing between function parameters.

## Commenting

- Use comments to explain code which cannot be written more clearly.

- Use comments to more clearly indicate different sections of code.

## Javascript Specifics

- Use semicolons wherever possible.

- Use double quotes (“), not single quotes (‘).

- When destructuring an object, add spaces before the first variable and after the last.

    ```
    const { firstName, lastName } = student;
    ```

- If an event handler is used only once and not particularly long or complex, use an inline arrow function

    ```
    onClick={() => navigate(“/login”)}
    ```
