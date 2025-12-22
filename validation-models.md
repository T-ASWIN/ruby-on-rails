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

///
class Invoice < ApplicationRecord
  validate :expiration_date_cannot_be_in_the_past,
    :discount_cannot_be_greater_than_total_value

  def expiration_date_cannot_be_in_the_past
    if expiration_date.present? && expiration_date < Date.today
      errors.add(:expiration_date, "can't be in the past")
    end
  end

  def discount_cannot_be_greater_than_total_value
    if discount > total_value
      errors.add(:discount, "can't be greater than total value")
    end
  end
end
