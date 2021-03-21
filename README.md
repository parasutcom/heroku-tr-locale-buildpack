# heroku-tr-locale-buildpack

This buildpack is used in https://github.com/parasutcom/server repository to add `tr_TR` collation to test databases. `heroku/ruby` buildpack runs `rake db:schema:load_if_ruby` as part of the buildpack script, which creates database objects from `schema.rb` file. `tr_TR` collation is not included in Heroku runtime by default and table columns which use this collation cause the test setup to fail.

Heroku suggests the usage of https://github.com/heroku/heroku-buildpack-locale buildpack. However that buildpack doesn't actually install the locale system-wide as stated in https://github.com/heroku/heroku-buildpack-locale/issues/13#issuecomment-629075405. Therefore `CREATE COLLATION` statement fails.

To fix this, we clone `en_EN` collation with the name `tr_TR`. This clone does not actually behave like the `tr_TR` collation, but we trick Postgres into believing that it does have such a collation. This way test setup does not fail.

