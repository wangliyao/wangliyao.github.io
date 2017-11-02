### 在rails 4.2.2中,执行rake db:migrate 遇到报错pg_dump: invalid option -- i

 ```ruby
 `pg_dump -i -s -x -O -f #{Shellwords.escape(filename)} #{search_path} #{Shellwords.escape(config['database'])}`
 ```
 * rails 4.2.2中连接pg数据库 遇到pg_dump: invalid option -- i，google一下说是因为这一句的原因 db:structure:dump task
 * 解决方案为，在initializers重新配置一个不同的ActiveRecord方法，合并起来创建一个新的db:structure:dump task ,当运行rake db的时候执行所创建的新任务
 ```ruby
 Rake::Task["db:structure:dump"].clear
namespace :db do
  namespace :structure do
    desc "Overriding the task db:structure:dump task to remove -i option from pg_dump to make postgres 9.5 compatible"
    task dump: [:environment, :load_config] do
      config = ActiveRecord::Base.configurations[Rails.env]
      set_psql_env(config)
      filename =  File.join(Rails.root, "db", "structure.sql")
      database = config["database"]
      command = "pg_dump -s -x -O -f #{Shellwords.escape(filename)} #{Shellwords.escape(database)}"
      raise 'Error dumping database' unless Kernel.system(command)

      File.open(filename, "a") { |f| f << "SET search_path TO #{ActiveRecord::Base.connection.schema_search_path};\n\n" }
      if ActiveRecord::Base.connection.supports_migrations?
        File.open(filename, "a") do |f|
          f.puts ActiveRecord::Base.connection.dump_schema_information
          f.print "\n"
        end
      end
      Rake::Task["db:structure:dump"].reenable
    end
  end

  def set_psql_env(configuration)
    ENV['PGHOST']     = configuration['host']          if configuration['host']
    ENV['PGPORT']     = configuration['port'].to_s     if configuration['port']
    ENV['PGPASSWORD'] = configuration['password'].to_s if configuration['password']
    ENV['PGUSER']     = configuration['username'].to_s if configuration['username']
  end
end
  ```
  
  具体回答 ![“pg_dump: invalid option — i” when migrating]（https://stackoverflow.com/questions/35999906/pg-dump-invalid-option-i-when-migrating）
