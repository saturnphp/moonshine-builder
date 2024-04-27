![logo](https://github.com/moonshine-software/moonshine/raw/2.x/art/lego.png)

## Creating projects using schemas for the [MoonShine](https://github.com/moonshine-software/moonshine).

#### Hello, Laravel and MoonShine User!

This package allows you to describe the entire project structure using a JSON schema and generate necessary files such as:
<ul>
    <li>Models</li>
    <li>Migrations</li>
    <li>Resources</li>
</ul>

### Installation:
```shell
composer require dev-lnk/moonshine-builder
```
### Configuration:
Publish the package configuration file:
```shell
php artisan vendor:publish --tag=moonshine-builder
```
In the configuration file, specify the path to your JSON schemas:

```php
return [
    'builds_dir' => base_path('builds')
];
```

Now you can run the command:

```shell
php artisan moonshine:build
```
You will be given options as to which scheme to use when generating the code, form example:

![img1](https://raw.githubusercontent.com/dev-lnk/moonshine-builder/master/examples/img_1.png)

![img2](https://raw.githubusercontent.com/dev-lnk/moonshine-builder/master/examples/img_2.png)

![img3](https://raw.githubusercontent.com/dev-lnk/moonshine-builder/master/examples/img_3.png)

![img4](https://raw.githubusercontent.com/dev-lnk/moonshine-builder/master/examples/img_4.png)

### Creating a Schema
In the <code>builds_dir</code> directory, create a schema file, for example, <code>category.json</code>:
```json
{
  "resources": [
    {
      "CategoryResource": {
        "fields": {
          "id": {
            "type": "id",
            "methods": [
              "sortable"
            ]
          },
          "title": {
            "type": "string",
            "name": "Название"
          }
        }
      }
    }
  ]
}
```
To generate project files, run the command:
```shell
 php artisan moonshine:build category.json
```
A more detailed example with multiple resources and relationships can be found [here](https://github.com/dev-lnk/moonshine-builder/blob/master/examples/project.json).
### Creation from sql table
You can create a resource using a table schema.You must specify the table name and select <code>table</code> type. Example:
```shell
 php artisan moonshine:build users --type=table
```
Result:
```php
public function fields(): array
{
    return [
        Block::make([
            ID::make('id'),
            Text::make('Name', 'name'),
            Text::make('Email', 'email'),
            Date::make('EmailVerifiedAt', 'email_verified_at'),
            Text::make('Password', 'password'),
            Text::make('RememberToken', 'remember_token'),
        ]),
    ];
}
```

After generating the files, make sure to register all new Resources in your <code>MoonShineServiceProvider</code>

### Timestamps
You can specify the timestamp: true flag
```json
{
  "resources": [
    {
      "CategoryResource": {
        "timestamps": true,
        "fields": {
        }
      }
    }
  ]
}
```
The created_at and updated_at fields will be added to your code. If you manually specified the created_at and updated_at fields, the `timestamps` flag will be automatically set to true

### Soft deletes
Works similarly to the `timestamps` flag and the `deleted_at` field

### Be careful, at the moment all resource and model files are overwritten during generation!
