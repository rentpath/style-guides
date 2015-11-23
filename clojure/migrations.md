#Database Migrations

## migration name:
 - Does not clash with previous migrations (numbers must increment).
 - Describes whether it is adding a new object or altering an old one.
 - identifies which object is affected.

## Up Function:
 - Affects only the appropriate fields.
 - Uses the correct data types.
 - If it is adding a constraint, up handles invalid data.

## Down Function:
 - if the up created an object, down removes it.
 - if up altered an object, drop restores it to previous form.
 - If up removed a constraint, down handles invalid data.

## Testing Migrations:
 - Migrate up, then down, then up
 - Integration tests should pass when migration applied and fail when not applied.

## Gotchas
 - If a tool was used to generate the alter script, verify that the database name is not included in the create/alter statement.
 - If the database uses schemas, verify the proper schema was specfied.
 - To verify that the current migration can be applied after those already exising on the CI database, modify environment settings to point to CI, and run `lein joplin pending <db>` This will cause an error if the current migration name comes before one that is already applied.
