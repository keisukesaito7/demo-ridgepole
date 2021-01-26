# Ridgepole demo

## initial settings

```
# create new app

$ rails _6.0.0_ new demo-ridgepole --skip-test -d mysql
$ cd demo-ridgepole
$ rails db:create
```

## install gem ridgepole

```ruby
# Gemfile

gem 'ridgepole'
```

```
$ bundle install
```

## create User table

```
# db/Schemafile

create_table "users", force: true do |t|
  t.string   "email"
  t.string   "name"
  t.datetime "created_at"
  t.datetime "updated_at"
end
```

```
$ bundle exec ridgepole -c config/database.yml -E development --apply -f db/Schemafile
```

## create model

```
$ rails g model user --skip-migration
```

## create user (test)

```
irb(main):001:0> User.create(email: "test@gmail.com", name: "test_user_1")
   (0.2ms)  BEGIN
   (0.2ms)  SAVEPOINT active_record_1
  User Create (1.3ms)  INSERT INTO `users` (`email`, `name`, `created_at`, `updated_at`) VALUES ('test@gmail.com', 'test_user_1', '2021-01-26 13:01:16', '2021-01-26 13:01:16')
   (0.2ms)  RELEASE SAVEPOINT active_record_1
=> #<User id: 2, email: "test@gmail.com", name: "test_user_1", created_at: "2021-01-26 13:01:16", updated_at: "2021-01-26 13:01:16">
```

## add null: false

```
irb(main):003:0> User.create(name: "test")
   (0.3ms)  SAVEPOINT active_record_1
  User Create (1.4ms)  INSERT INTO `users` (`name`, `created_at`, `updated_at`) VALUES ('test', '2021-01-26 13:05:51', '2021-01-26 13:05:51')
   (0.3ms)  ROLLBACK TO SAVEPOINT active_record_1
Traceback (most recent call last):
        1: from (irb):3
ActiveRecord::NotNullViolation (Mysql2::Error: Field 'email' doesn't have a default value)
```

## add validates :name, presence: true

```
irb(main):004:0> User.create(email: "test")
=> #<User id: nil, email: "test", name: nil, created_at: nil, updated_at: nil>
```
