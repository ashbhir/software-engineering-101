# Software Engineering 101

### Algo DS

### OOPS Principles

### DBMS

### Operating Systems

### Computer Networks

### Software Testing

### Compiler Design

### [Design Patterns](#design-patterns-1)

### [Agile Philosophy](#Agile-Manifesto)

### Machine Learning

# Design Patterns

## Solid Principles

### Single Responsibility Principle
```
A class should have one and only one reason to change, meaning that a class should have only one job.
```

```
class Employee {
  fun getTax() {
    val tax = this.salary * 10/100;
    FileReader f = new FileReader();
    f.out(tax)
    print(tax)
  }
}
```
Employee class is handling responsibilities of calculating tax, outputing to file and printing to console.

We should rather make a separate class as `TaxCalculator`, `Logger`, `FileOutputter` etc.

### Open-Closed Principle
```
Objects or entities should be open for extension but closed for modification.
```
```
class Bird {
  fun fly() {
    if (this.type == 'flamingo') {
      // do something
    } else if (this.type == 'egret') {
      // do something else
    }
  }
}
```
Here, function fly and in effect class Bird will be modified on every addition of a type of bird. We should rather have separate bird classed inheriting Bird class.

```
class Flamingo extends Bird {
  fun fly() {}
}

class Egret extends Bird {
  fun fly() {}
}
```

Also, for class `TaxCalculator` we can different types of Employees implement different TaxCalculator like `TaxCalculatorIntern`, `TaxCalculatorFullTime` or `TaxClaculatorCEO`.
**Ques**: But how do we instantiate different TaxCalculator classes for different Employess ?
**Ans**: We use Factory Pattern. 

```
class Employee {
  fun calculateTax() {
    val taxCalculator = TaxCalculator.create(this.type)
    return taxCalculator.calculate()
  }
}
```

### Liskov Substitution Principle
```
Every subclass or derived class should be substitutable for their base or parent class.
```
```
class Penguin extends Bird {
  fun fly() {
    // what to do ??
  }
}
```
Not all birds can fly. It doesn't make sense for Bird class to contain fly method if not all birds are flyable. Perhaps make an interface `Flyable` and we can only call bird.fly() on bird instances that implement `Flyable` interface.

```
class Stack extends ArrayList<Integer> {
  fun push() {}
  fun pop() {}
}
```
Doesn't make sense Stack class to extend ArrayList as it will have to implement all methods of ArrayList and throw error. One can access any element of the stack which is not the desired behavior. We should favor composition here.

### Interface Seggregation Principle
```
A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use.
```
This simply means Single Responsible Principle for interfaces. `Flayable` interface shouldn't have a `flapWings()` method but rather make a new interface `Flappable`.

### Dependency Inversion Principle
```
Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.
```
```
class ProductSQLRepo {
  fun save() {}
  fun get() {}
}

class Payment {
  fun pay(productId: Int) {
    val sqlRepo = new ProductSQLRepo(productId)
    val product = sqlRepo.get()
    payAmount(product.price)
  }
}
```
Here high-level class `Payment` is dependent on a low-level module `ProductSQLRepo` which is not correct for two reasons- If later, product fetching is changed to ProductMongoRepo, a lot of changes will be required + Payment should not be concerned with initialisation of the repo. 

It is solved by following dependency injection and leaving reposibility of instantiating of repo on the caller.

```
abstract class ProductRepo {
  fun save() {}
  fun get() {}
}

class Payment {
  fun pay(productId: Int, productRepo: ProductRepo) {
    val product = productRepo.get()
    payAmount(product.price)
  }
}
```


IOC (Inversion of Control) frameworks like springboot help with managing dependencies.

### Compostion and Inheritance

Both are generalisations that have a `has a` relation rather than `is a` relation in class inheritance. 

Compostions are a `belong to` type of relationship. Class `Car` is composed of class `Engine`.
Aggregations are a `has a` type of relationship. Class `Doctor` may or may not contain class `Patient`.


# Agile Manifesto

### The Four Values of The Agile Manifesto
The Agile Manifesto is comprised of four foundational values and 12 supporting principles which lead the Agile approach to software development. Each Agile methodology applies the four values in different ways, but all of them rely on them to guide the development and delivery of high-quality, working software.

1. **Individuals and Interactions Over Processes and Tools**:
The first value in the Agile Manifesto is “Individuals and interactions over processes and tools.” Valuing people more highly than processes or tools is easy to understand because it is the people who respond to business needs and drive the development process. If the process or the tools drive development, the team is less responsive to change and less likely to meet customer needs. Communication is an example of the difference between valuing individuals versus process. In the case of individuals, communication is fluid and happens when a need arises. In the case of process, communication is scheduled and requires specific content.

2. **Working Software Over Comprehensive Documentation**:
Historically, enormous amounts of time were spent on documenting the product for development and ultimate delivery. Technical specifications, technical requirements, technical prospectus, interface design documents, test plans, documentation plans, and approvals required for each. The list was extensive and was a cause for the long delays in development. Agile does not eliminate documentation, but it streamlines it in a form that gives the developer what is needed to do the work without getting bogged down in minutiae. Agile documents requirements as user stories, which are sufficient for a software developer to begin the task of building a new function.
The Agile Manifesto values documentation, but it values working software more.

3. **Customer Collaboration Over Contract Negotiation**:
Negotiation is the period when the customer and the product manager work out the details of a delivery, with points along the way where the details may be renegotiated. Collaboration is a different creature entirely. With development models such as Waterfall, customers negotiate the requirements for the product, often in great detail, prior to any work starting. This meant the customer was involved in the process of development before development began and after it was completed, but not during the process. The Agile Manifesto describes a customer who is engaged and collaborates throughout the development process, making. This makes it far easier for development to meet their needs of the customer. Agile methods may include the customer at intervals for periodic demos, but a project could just as easily have an end-user as a daily part of the team and attending all meetings, ensuring the product meets the business needs of the customer.

4. **Responding to Change Over Following a Plan**:
Traditional software development regarded change as an expense, so it was to be avoided. The intention was to develop detailed, elaborate plans, with a defined set of features and with everything, generally, having as high a priority as everything else, and with a large number of many dependencies on delivering in a certain order so that the team can work on the next piece of the puzzle.

 

With Agile, the shortness of an iteration means priorities can be shifted from iteration to iteration and new features can be added into the next iteration. Agile’s view is that changes always improve a project; changes provide additional value.

Perhaps nothing illustrates Agile’s positive approach to change better than the concept of Method Tailoring, defined in An Agile Information Systems Development Method in use as: “A process or capability in which human agents determine a system development approach for a specific project situation through responsive changes in, and dynamic interplays between contexts, intentions, and method fragments.” Agile methodologies allow the Agile team to modify the process and make it fit the team rather than the other way around.

### The Twelve Agile Manifesto Principles
The Twelve Principles are the guiding principles for the methodologies that are included under the title “The Agile Movement.” They describe a culture in which change is welcome, and the customer is the focus of the work. They also demonstrate the movement’s intent as described by Alistair Cockburn, one of the signatories to the Agile Manifesto, which is to bring development into alignment with business needs.

The twelve principles of agile development include:

- Customer satisfaction through early and continuous software delivery – Customers are happier when they receive working software at regular intervals, rather than waiting extended periods of time between releases.
- Accommodate changing requirements throughout the development process – The ability to avoid delays when a requirement or feature request changes.
Frequent delivery of working software – Scrum accommodates this principle since the team operates in software sprints or iterations that ensure regular delivery of working software.
- Collaboration between the business stakeholders and developers throughout the project – Better decisions are made when the business and technical team are aligned.
- Support, trust, and motivate the people involved – Motivated teams are more likely to deliver their best work than unhappy teams.
- Enable face-to-face interactions – Communication is more successful when development teams are co-located.
- Working software is the primary measure of progress – Delivering functional software to the customer is the ultimate factor that measures progress.
- Agile processes to support a consistent development pace – Teams establish a repeatable and maintainable speed at which they can deliver working software, and they repeat it with each release.
- Attention to technical detail and design enhances agility – The right skills and good design ensures the team can maintain the pace, constantly improve the product, and sustain change.
- Simplicity – Develop just enough to get the job done for right now.
- Self-organizing teams encourage great architectures, requirements, and designs – Skilled and motivated team members who have decision-making power, take ownership, communicate regularly with other team members, and share ideas that deliver quality products.
- Regular reflections on how to become more effective – Self-improvement, process improvement, advancing skills, and techniques help team members work more efficiently.

# References

- [ ] Read "Read Algo DS Summary"
- [ ] Read "Read OOPS Principles Summary"
- [ ] Solve top medium questions on leetcode
- [ ] Read "DBMS Summary"
- [ ] Read "Operating Systems Summary"
- [ ] Read "Networking Summary"
- [ ] Read "Machine Learning Summary"
