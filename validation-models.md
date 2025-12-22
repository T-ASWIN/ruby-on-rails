class MyValidator < ActiveModel::Validator
  def validate(record)
    unless record.name.start_with? "X"
      record.errors.add :name, "Provide a name starting with X, please!"
    end
  end
end

class Person < ApplicationRecord
  validates_with MyValidator
end


we will do at ActiveModel::Validator
