# Solid_Priciples in C++

## Introduction
In this article, we'll dive into, probably, the most famous 5 Object-Oriented design principles using C++ examples.

Please, note that the classes written here aren't complete classes by all means, they might be greatly simplified 
for the sake of a more clear picture about the use case of each design principle, so things like virtual destructors,
move constructors and so on will be ignored.

## What is S.O.L.I.D?
SOLID is an acronym for the first five object-oriented design (OOD) principles by Robert C. Martin, known as "Uncle Bob."

Those design principles can be seen as guidelines for any object-oriented design that when followed, help 
produce maintainable and extensible code.

The following is a list of those design principles that make the acronym "SOLID":
# 1. Single Responsibility Principle
# 2. Open Closed Principle
# 3. Liskov Substitution Principle
# 4. Interface Segregation Principle
# 5. Dependency Inversion Principle
So, let's get started at demystifying each one of those principles.

## Single Responsibility Principle
This principle suggests that a class should have only a single responsibility. In other words, a class should only have one purpose 
and shouldn't do more than just that purpose, so when a change in this purpose is needed, we have exactly one class to modify.

A good example of that would be to have countries around the globe as classes, where each country has its average citizen 
income as a member, as in the figure below.

```cpp
class UnitedStates {
    uint16_t avg_citizen_income;
    public:
    UnitedStates(uint16_t avg_income) : avg_citizen_income(avg_income)
    {
    }
};

class UnitedKingdom {
    unit16_t avg_citizen_income;
    public:
    UnitedKingdom(uint16_t avg_income) : avg_citizen_income(avg_income)
    {
    }
};
```
