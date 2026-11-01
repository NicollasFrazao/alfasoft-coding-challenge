# Alfasoft Challenge - Contact management (Laravel CRUD)

A Laravel 10 web app for managing contacts (name, e-mail, phone). It uses an AdminLTE 3 layout, a server-side DataTable and soft deletes, and anyone can view the list while every other page needs a login.

## Context

Solution to the hiring challenge of Alfasoft (2025). The challenge statement is kept in Portuguese in [`CHALLENGE.md`](CHALLENGE.md). In short: a CRUD app for contacts with a public list, login for every other action, soft deletes, unique e-mail and phone, and tests for form-validation errors.

## What it does

- **Public list** at `/contacts` (`/` redirects there). It is a server-side Yajra DataTable with id, name, e-mail and phone. The actions column (view, edit, delete) appears only for logged-in users.
- **Authenticated pages** (`auth` middleware):
  - `GET /contacts/create` and `POST /contacts`: create a contact.
  - `GET /contacts/{contact}`: a full detail page with every field plus edit and delete buttons.
  - `GET /contacts/{contact}/edit` and `PUT|PATCH /contacts/{contact}`: update a contact.
  - `DELETE /contacts/{contact}`: soft delete (`SoftDeletes` on `Contact`).
- **Validation** (`app/Http/Requests/ContactRequest.php`):
  - `name`: required, at least 6 characters.
  - `email`: required, `email:rfc,dns`, unique.
  - `phone`: required, exactly 9 characters, unique.
  - On update, the uniqueness checks skip the contact being edited.
- **Authentication** comes from `laravel/ui` (`Auth::routes()`). `DatabaseSeeder` creates a static admin user.
- Writes run inside a database transaction, and the result is shown as a flash message (`laracasts/flash`). The interface is in Portuguese.

## Tech stack

Versions are the ones locked in `composer.lock`.

- PHP `^8.1`, Laravel 10.48
- laravel/ui 4.6 (auth scaffolding), laravelcollective/html 6.4 (forms)
- yajra/laravel-datatables 10.0 (laravel-datatables-oracle 10.11)
- laracasts/flash 3.2
- AdminLTE 3.2.0, served as static files from `public/dist` and `public/plugins`
- MySQL (`.env.example`); the tests use in-memory SQLite (`phpunit.xml`)
- PHPUnit 10.5

## Running locally

Requirements: PHP 8.1+ with `pdo_mysql` (and `pdo_sqlite` for the tests), Composer, and MySQL.

```bash
composer install
cp .env.example .env              # then set DB_DATABASE / DB_USERNAME / DB_PASSWORD
php artisan key:generate
php artisan migrate --seed        # creates the tables and the admin user
php artisan serve                 # http://127.0.0.1:8000
```

The admin credentials are in `database/seeders/DatabaseSeeder.php`.

The layout loads front-end assets that are already built and committed: `public/js/app.js` and `public/css/app.css` through `mix('js/app.js')` and `public/mix-manifest.json`, plus the AdminLTE plugins. You do not need `npm` to run the app. The Vite setup in `package.json` is not referenced by any view.

## Tests

```bash
php artisan test
```

`tests/Feature/ContactTest.php` has four tests (with `RefreshDatabase`):

1. Creating a contact with invalid data returns errors for name, e-mail and phone, and nothing is stored.
2. Creating a contact with valid data redirects and stores it.
3. Updating a contact with another contact's e-mail and phone returns errors for both.
4. Updating a contact with its own e-mail and phone succeeds.

Checked with PHP 8.3: `4 passed (12 assertions)`.

The valid-data tests go through the `email:dns` rule, so they need DNS resolution (network access).

## Project structure

```
app/Http/Controllers/ContactController.php   CRUD actions
app/Http/Requests/ContactRequest.php         Validation rules
app/DataTables/ContactDataTable.php          Server-side DataTable for the index
app/Models/Contact.php                       Eloquent model with SoftDeletes
resources/views/contacts/                    index, create, edit, show, fields, row actions
resources/views/layouts/                     AdminLTE layout, navbar, sidebar
database/migrations/..._create_contacts_table.php
tests/Feature/ContactTest.php
```

## Notes

- `vendor/` and `node_modules/` are not committed. The AdminLTE distribution is: about 2,050 files under `public/plugins` and `public/dist`, plus one published DataTables script in `public/vendor/datatables/`.
- E-mail and phone uniqueness lives only in validation. The migration has no unique index.
- The unique rules count soft-deleted rows, so a deleted contact's e-mail or phone cannot be reused.
- The phone rule checks the length (9 characters), not that every character is a digit.
- `Auth::routes()` turns on public registration as well as login.
- The `/` route sends a redirect response with status 200. Browsers move on only because of the meta refresh in the response body.
- `ContactTest` sits in `tests/Feature` but declares the namespace `Tests\Unit`.
