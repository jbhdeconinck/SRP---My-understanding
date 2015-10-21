#Single responsibility principle

The SRP states that a class should have only one responsibility and should fully execute it – that responsibility shouldn’t be splattered among more than one class.

#Encapsulation

An object is not well encapsulated when its behavior can be affected without calling its user interface.

Another way to think about it is that a well encapsulated object draws a solid boundary or wall around itself, and ensures that all the code that changes its behavior is contained within the object itself (i.e. not in another object).

A good example: 

## Poor encapsulation:

``` Ruby

class TrainTicketStation
  def initialize(train)
    @train = train
  end

  def buy_ticket
    available_seats = @train.seats.reject { |seat| seat.reserved? }
    raise "TrainIsFull" if available_seats.length < 1
    available_seats.first.reserved = true
  end
end

class TrainTicketWebsite
  def initialize(train)
    @train = train
  end

  def buy_tickets(number_of_seats)
    available_seats = @train.seats.reject { |seat| seat.reserved? }
    raise "InsuffecientSeats" if available_seats.length < number_of_seats
    seats_to_reserve = available_seats.slice(0, number_of_seats)
    seats_to_reserve.each { |seat| seat.reserved = true }
  end
end

```

## Better encapsulation

```

Involves creating a new Train class. [This example also illustrate the SRP]


class TrainTicketStation
  def initialize(train)
    @train = train
  end

  def buy_ticket
    @train.buy_tickets(1)
  end
end

class TrainTicketWebsite
  def initialize(train)
    @train = train
  end

  def buy_tickets(number_of_seats)
    @train.buy_tickets(number_of_seats)
  end
end

class Train
  def buy_tickets(amount)
    raise "InsuffecientSeats" if available_seats.length < amount
    seats_to_reserve = available_seats.slice(0, number_of_seats)
    seats_to_reserve.each { |seat| seat.reserved = true }
  end

  private
  def available_seats
    @seats.reject { |seat| seat.reserved? }
  end
end
```
