#include <iostream>
#include <vector>
#include <string>
#include <limits>
using namespace std;
struct House {
    double price;
    int size;
    int bedrooms;
    string city;
    double price_size_ratio;

    House(double p, int s, int b, std::string c)
        : price(p), size(s), bedrooms(b), city(c) {
        price_size_ratio = price / size; 
    }

    void display() const {
        cout << "Price: " << price << ", Size: " << size
            << " sqft, Bedrooms: " << bedrooms
            << ", City: " << city << endl;
    }
};

vector<House> find_houses(const vector<House>& houses,
    double max_price,
    int min_bedrooms,
    const string& city) {
    vector<House> results;
    for (const auto& house : houses) {
        if ((max_price == -1 || house.price <= max_price) &&
            (min_bedrooms == -1 || house.bedrooms >= min_bedrooms) &&
            (city == "?" || house.city == city)) {
            results.push_back(house);
        }
    }
    return results;
}

House find_lowest_priced_house(const std::vector<House>& houses) {
    House lowest_priced_house = houses[0];
    for (const auto& house : houses) {
        if (house.price < lowest_priced_house.price) {
            lowest_priced_house = house;
        }
    }
    return lowest_priced_house;
}

House find_largest_house(const vector<House>& houses) {
    House largest_house = houses[0];
    for (const auto& house : houses) {
        if (house.bedrooms > largest_house.bedrooms) {
            largest_house = house;
        }
    }
    return largest_house;
}

House find_best_ratio_house(const std::vector<House>& houses) {
    House best_ratio_house = houses[0];
    for (const auto& house : houses) {
        if (house.price_size_ratio < best_ratio_house.price_size_ratio) {
            best_ratio_house = house;
        }
    }
    return best_ratio_house;
}

int main() {
    vector<House> houses = {
        House(200000, 1500, 3, "New York"),
        House(300000, 2000, 4, "Los Angeles"),
        House(250000, 1800, 3, "New York"),
        House(400000, 2200, 5, "Chicago"),
    };

    double max_price;
    int min_bedrooms;
    string city;

    cout << "Enter maximum price (or -1 for no preference): ";
    cin >> max_price;
    cout << "Enter minimum bedrooms (or -1 for no preference): ";
    cin >> min_bedrooms;
    cout << "Enter city (or ? for no preference): ";
    cin >> city;

    vector<House> matches = find_houses(houses, max_price, min_bedrooms, city);

    cout << "\nMatching houses:\n";
    if (matches.empty()) {
        cout << "No houses found matching the criteria." <<endl;
    }
    else {
        for (const auto& house : matches) {
            house.display();
        }
    

    cout << "\nAdditional search options:\n";
    House lowest_priced_house = find_lowest_priced_house(houses);
    cout << "House with the lowest price:\n";
    lowest_priced_house.display();
    House largest_house = find_largest_house(houses);
    cout << "\nLargest house (by bedrooms):\n";
    largest_house.display();
    House best_ratio_house = find_best_ratio_house(houses);
   cout << "\nHouse with the best price/size ratio:\n";
    best_ratio_house.display();

    return 0;
}
