# Solid_Priciples in C++

## Introduction
In this article, we'll dive into the most famous 5 Object-Oriented design principles using C++ examples.

Please, note that the classes written here aren't complete classes by all means, they might be greatly simplified for the sake of a more clear picture about the use case of each design principle, so things like virtual destructors, move constructors and so on will be ignored.

## What is S.O.L.I.D?
SOLID is an acronym for the first five object-oriented design (OOD) principles by Robert C. Martin, known as "Uncle Bob."

Those design principles can be seen as guidelines for any object-oriented design that when followed, help produce maintainable and extensible code.

The following is a list of those design principles that make the acronym "SOLID":
### 1. Single Responsibility Principle
### 2. Open Closed Principle
### 3. Liskov Substitution Principle
### 4. Interface Segregation Principle
### 5. Dependency Inversion Principle
So, let's get started at demystifying each one of those principles.

## Single Responsibility Principle

This principle suggests that a class should have only a single responsibility. In other words, a class should only have one purpose and shouldn't do more than just that purpose, so when a change in this purpose is needed, we have exactly one class to modify.

A good example of that would be to have countries around the globe as classes, where each country has its average citizen income as a member, as seen below.

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

Say we need to calculate the average tax from that average citizen income, a bad approach contradicting the single responsibility principle would be to give each class its own "CalcTax" member function. A good design approach, however, should be to make a tax calculator class. This way whenever the tax calculation process needs a change, we only need modify the tax calculator class and not modify 195 classes (a class for each country). this can be seen below.

```cpp
class TaxCalculator;

class UnitedStates {
    friend TaxCalculator;
    uint16_t avg_citizen_income;
    public:
    UnitedStates(uint16_t avg_income) : avg_citizen_income(avg_income)
    {
    }
};

class UnitedKingdom {
    friend TaxCalculator;
    unit16_t avg_citizen_income;
    public:
    UnitedKingdom(uint16_t avg_income) : avg_citizen_income(avg_income)
    {
    }
};

class TaxCalculator {
    public:
    uint16_t CalculateTax(UnitedKingdom country) {
        return country.avg_citizen_income / 10;
    }
    
    uint16_t CalculateTax(UnitedStates country) {
        return country.avg_citizen_income / 8;
    }
};
```

So, as you can see the strategy for the calculation of each tax can be different from one country to another, this TaxCalculator class could easily account for that. Assuming a change in the policy of the taxes in a country, modifying the code to account for that's takes only modifying one function, which is the case if each country had its CalcuateTax member function too. What the single responsibility principle really added, was that modifying a policy, doesn't really modify the whole country class, which could be time consuming in a more realistic complex project.

## Open Closed Priciple

This principle states that your code should generally be open for extension, yet closed for modification. Which sounds like Gibberish and it will continue to sound so until demystified with an example, so let's get started.

In the upcoming example, let's say we have a class named "TextAdder" that has a member function called "AddToText" which takes two strings and appends the second to the first, this can be seen below.

```cpp
class TextAdder{
    public:
    std::string AddToText(const std::string& original,
        const std::string& new_string,TextAddType& adder){
            return adder.add(original , new_string);
        }
};
```

Now, say we want to add another functionality to the TextAdder, for example adding the functionality of adding the new string before the original one. Extending the class through adding another function named "AddBeforeText" is the wrong way to extend the functionalities of the class, since we had to modify the class.

Following the Open/Closed Principle, this class should be extended making the AddToText function take a third argument specifying what kind of string addition the user wants done. This can be done using polymorphism. A base class is passed to the "AddToText" function, then its own "add" function is called. Let's implement that.

```cpp
class TextAddType{
    public:
    virtual std::string add(const std::string& original,
        const std::string& new_string) = 0;
};

class TextAdder{
    public:
    std::string AddToText(const std::string& original,
        const std::string& new_string,TextAddType& adder){
            return adder.add(original , new_string);
        }
};

class TextAppend : public TextAddType{
    public:
    std::string add(const std::string& original,
        const std::string& new_string) override{
            return original + new_string;
        }
};

class TextPrepend : public TextAddType{
    public:
    std::string add(const std::string& original,
        const std::string& new_string) override{
            return new_string + original;
        }
};
```

By looking at the above code, we see that the "TextAdder" class, which is the class designed to be interfaced the most by the user, has become open for extension, but closed for modification. We can easily add another class adding the functionality of adding the new string after the third letter of the originl string and so on.

## Liskov Substitution Principle

This principle states that every derived class should be substitutable for their base class. In other words, derived classes should behave correctly, as expected and avoid logical errors due to their parent class.

Say we have a Rectangle class, which is a basic class that has width and height attributes, set and get width functions, set and get height functions and a calculate area function, as seen below.

```cpp
class Rectangle{
    protected:
    uint16_t Width;
    uint16_t Height;

    public:
    Rectangle(uint16_t width, uint16_t height): Width(width), Height(height){}
    virtual void SetWidth(uint16_t width){
        Width = width;
    }
    virtual void SetHeight(uint16_t height){
        Height = height;
    }
    virtual uint16_t GetWidth(){return Width;}
    virtual uint16_t GetHeight(){return Height;}
    virtual uint32_t CalculateArea(){return Width*Height;}
};
```

If we want to add a Square subclass based on this class, the new class will go like this.

```cpp
class Square : public Rectangle{
    public:
    Square(uint16_t L): Rectangle(L, L){}
    void SetWidth(uint16_t width) override{
         Width = width; Height = width;
         }
    void SetHeight(uint16_t height) override{
         Width = height; Height = height;
         }
    uint16_t GetWidth() override{return Width;}
    uint16_t GetHeight() override{return Height;}
};
```

So, what's the problem with that code? Isn't the square correctly handling the dimension inequality case?

Well, the problem is that the user using "SetWidth" and "SetHeight" functions might not get that they change the other attribute, so the user might get unexpected results when calling "CalculateArea" function.

So, to sum it up, This Square class isn't substitutable for the Rectangle class, and thereby breaks Liskov principle.

## Interface Segregation Principle

This principle states that a client should never be forced implement an interface that it doesn't use. In other words, If a class has a function public to the user, it must be callable.

Back to our square example, one might think that adding a "SetLenght" and "GetLenght" functions, while making the other width and height related functions empty or even warn the user somehow, is a good approach. This can be seen below.

```cpp
class Square : public Rectangle{
    public:
    Square(uint16_t L): Rectangle(L, L){}
    void SetWidth(uint16_t width) override{(void)width;}
    void SetHeight(uint16_t height) override{(void)height;}
    void SetLength(uint16_t L){ Width=L; Height=L;}
    uint16_t GetLength(){return Width;}
    uint16_t GetWidth() override{return GetLength();}
    uint16_t GetHeight() override{return GetLength();}
};
```

Now, the "SetWidth" and "SetHeight" functions do, basically nothing, while the "GetWidth" and "GetHeight" functions call "GetLength" under the hood. So, what's the problem witht this design? Does our square class follow Liskov principle, now?

Actually, our class follows it indeed, by making the square class not behave in an unexpected way as the Width and height functions have been emptied, they could also be redesigned to print a warning or throw an exception when used. Still, this square class provides interfaces that aren't usable by the user, which is confusing and requires wasting lots of time both reading and writing the accompanying documentation.

A better approach would be to redesign the whole thing by simply making a 2D simple shape base class that has all the basic functionalities, then each child class adds its own specific functions. That way each class has no unused interfaces and follows the interface segregation principle. This can be seen in the code below.

```cpp
class TwoDSimpleShape{
    protected:
    uint16_t Width;
    uint16_t Height;
    public:
    TwoDSimpleShape(uint16_t width, uint16_t height): Width(width), Height(height){}
    virtual uint32_t CalculateArea(){return Width*Height;}
};

class Rectangle : public TwoDSimpleShape{
    public:
    Rectangle(uint16_t width, uint16_t height): TwoDSimpleShape(width, height){}
    void SetWidth(uint16_t width){Width = width;}
    void SetHeight(uint16_t height){Height = height;}
    uint16_t GetWidth(){return Width;}
    uint16_t GetHeight(){return Height;}
    
};

class Square : public TwoDSimpleShape{
    public:
    Square(uint16_t L): TwoDSimpleShape(L, L){}
    void SetLength(uint16_t L){ Width=L; Height=L;}
    uint16_t GetLength(){return Width;}
};
```

## Dependency Inversion Principle

This principle states that a higher level-module should not depend on a lower-level module, but they both should depend on abstractions. It also states that abstractions should not depend on details, but details should depend on abstractions. Since that sounds like some logic class in school, let's clarify with an example.

Suppose we have a "FruitBasket" class, which is a low-level module, where the user has "AddToBasket" functionality that appends a tuple of strings containing the fruit and the color to a vector.

Now suppose we have a high-level class named "ColorSeracher" that has a "ListColor" functionality, which goes through the vector item by item, then prints each found fruit with the matching color given to the function. This can be seen below.

```cpp
class FruitBasket { // Low-level module
    public:
    std::vector<std::pair<std::string, std::string>> Basket;
    void AddToBasket(const std::string& Fruit, const std::string& Color) {
        Basket.push_back(std::make_pair(Fruit, Color));
    }
};

class ColorSearcher{ // High-level module
    public:
    void ListColor(FruitBasket& fruit_basket, std::string color){
        for(auto item: fruit_basket.Basket){
            if (std::get<1>(item) == color) {
                std::cout<< "Found " << std::get<0>(item) << std::endl;
            }
        }
    }
};
```

The problem with this design is that the "ColorSeracher" high-level module depends heavily on the low-level module, since it knows the details about the "Basket" object found in it, as it knows it's a vector of tuples. Any modification to the low-level class, will now affect the higher-level one.

A simple solution would be to introduce an abstract class that both these modules depend on. By doing so, the high level class is seperated from the details. The code given below follows the dependency inversion principle.

```cpp
class BasketSearcher{   // abstract module
    public:
    virtual std::vector<std::string> SearchByColor(const std::string& color) = 0;
};

class FruitBasket: public BasketSearcher{ // Low-level module
    public:
    std::vector<std::tuple<std::string,std::string>> Basket;
    void AddToBasket(const std::string& Fruit, const std::string& Color){
        Basket.push_back(std::make_tuple(Fruit, Color));
    }
    std::vector<std::string> SearchByColor(const std::string& color) override{
        std::vector<std::string> Found;
        for(auto item: Basket){
            if (std::get<1>(item) == color){
                Found.push_back(std::get<0>(item));
            }
        }
    return Found;
    }
    
};

class ColorSearcher{ // High-level module
    public:
    void ListColor(BasketSearcher& basket,std::string color){
        for(auto item: basket.SearchByColor(color)){
            std::cout<< "Found " << item << std::endl;
        }
    }
};
```

Note that this design approach also follows the single responsibility principle, since it's the responsibility of the "FruitBasket" class to filter the items in it. Now, the high-level module doesn't depend on the lower-level one and it only knows that the "SearchByColor" member function returns a vector of strings of the found objects.





