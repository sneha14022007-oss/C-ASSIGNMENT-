1. Define a base class Crop with attributes like name, area, season, and yieldPerAcre. Use appropriate access specifiers and constructors

class Crop {
protected:
    string name, season;
    double area, yieldPerAcre;

public:
    Crop() {
        name = "Unknown";
        season = "Unknown";
        area = 0;
        yieldPerAcre = 0;
    }

    Crop(string n, double a, string s, double y) {
        name = n;
        area = a;
        season = s;
        yieldPerAcre = y;
    }

    double getTotalYield() const {
        return area * yieldPerAcre;
    }
};
2. Derive two classes CerealCrop and VegetableCrop from Crop using inheritance, adding their specific data members (e.g., fertilizer type, irrigation frequency).

class CerealCrop : public Crop {
private:
    string fertilizerType;

public:
    CerealCrop(string n, double a, string s,
               double y, string f)
        : Crop(n, a, s, y) {
        fertilizerType = f;
    }
};

class VegetableCrop : public Crop {
private:
    int irrigationFrequency;

public:
    VegetableCrop(string n, double a, string s,
                  double y, int i)
        : Crop(n, a, s, y) {
        irrigationFrequency = i;
    }
};
3. Use constructor overloading and a virtual destructor.

class Crop {
protected:
    string name, season;
    double area, yieldPerAcre;

public:
    Crop() {
        name = "Unknown";
        season = "Unknown";
        area = 0;
        yieldPerAcre = 0;
    }

    Crop(string n, double a, string s, double y) {
        if (a <= 0 || y < 0)
            throw invalid_argument("Invalid crop details");

        name = n;
        area = a;
        season = s;
        yieldPerAcre = y;
    }

    virtual ~Crop() {
        cout << "Crop object destroyed." << endl;
    }
};
4. Apply runtime polymorphism to calculate and display total yield for both crop types.

class Crop {
protected:
    string name;
    double area, yieldPerAcre;

public:
    Crop(string n, double a, double y)
        : name(n), area(a), yieldPerAcre(y) {}

    virtual double getTotalYield() const {
        return area * yieldPerAcre;
    }

    virtual void display() const {
        cout << "Crop: " << name << endl;
        cout << "Total Yield: "
             << getTotalYield() << endl;
    }

    virtual ~Crop() {}
};

class CerealCrop : public Crop {
public:
    CerealCrop(string n, double a, double y)
        : Crop(n, a, y) {}

    void display() const override {
        cout << "Cereal Crop" << endl;
        Crop::display();
    }
};

class VegetableCrop : public Crop {
public:
    VegetableCrop(string n, double a, double y)
        : Crop(n, a, y) {}

    void display() const override {
        cout << "Vegetable Crop" << endl;
        Crop::display();
    }
};

int main() {
    Crop* c1 = new CerealCrop("Wheat", 5, 20);
    Crop* c2 = new VegetableCrop("Tomato", 3, 15);

    c1->display();
    c2->display();

    delete c1;
    delete c2;

    return 0;
}
5. Demonstrate operator overloading to compare two crops based on yield.

class Crop {
protected:
    string name;
    double area, yieldPerAcre;

public:
    Crop(string n, double a, double y)
        : name(n), area(a), yieldPerAcre(y) {}

    double getTotalYield() const {
        return area * yieldPerAcre;
    }

    bool operator>(const Crop& other) const {
        return getTotalYield() > other.getTotalYield();
    }

    string getName() const {
        return name;
    }
};

int main() {
    Crop wheat("Wheat", 5, 20);
    Crop tomato("Tomato", 3, 15);

    if (wheat > tomato)
        cout << wheat.getName()
             << " has higher yield." << endl;
    else
        cout << tomato.getName()
             << " has higher or equal yield." << endl;

    return 0;
}
6. Create an abstract class Advisory that has a pure virtual function giveAdvice(). Implement this in derived classes to provide farming tips for different crops.

class Advisory {
public:
    virtual void giveAdvice() const = 0;

    virtual ~Advisory() {}
};

class CerealCrop : public Advisory {
public:
    void giveAdvice() const override {
        cout << "Maintain soil fertility and control weeds."
             << endl;
    }
};

class VegetableCrop : public Advisory {
public:
    void giveAdvice() const override {
        cout << "Monitor soil moisture and avoid overwatering."
             << endl;
    }
};

int main() {
    CerealCrop wheat;
    VegetableCrop tomato;

    wheat.giveAdvice();
    tomato.giveAdvice();

    return 0;
}

7. Use file handling to save and retrieve crop data.
   #include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main() {
    ofstream fout("crops.txt");

    if (!fout) {
        cout << "Error opening file for writing." << endl;
        return 1;
    }

    fout << "Wheat 5 Winter 20" << endl;
    fout << "Tomato 3 Summer 15" << endl;

    fout.close();

    cout << "Crop data saved successfully." << endl;

    ifstream fin("crops.txt");

    if (!fin) {
        cout << "Error opening file for reading." << endl;
        return 1;
    }

    string name, season;
    double area, yield;

    cout << "\nSaved Crop Data:\n";

    while (fin >> name >> area >> season >> yield) {
        cout << "Name: " << name
             << ", Area: " << area
             << ", Season: " << season
             << ", Yield per Acre: " << yield
             << endl;
    }

    fin.close();

    return 0;
}
8. Include at least one use of static members (e.g., count of crops).

#include <iostream>
using namespace std;

class Crop {
private:
    static int cropCount;

public:
    Crop() {
        cropCount++;
    }

    ~Crop() {
        cropCount--;
    }

    static int getCropCount() {
        return cropCount;
    }
};

int Crop::cropCount = 0;

int main() {
    Crop c1, c2, c3;

    cout << "Number of crops: "
         << Crop::getCropCount() << endl;

    return 0;
}
9. Ensure appropriate input/output formatting and proper exception handling (e.g.. invalid area or yield input).

#include <iostream>
#include <iomanip>
#include <stdexcept>
using namespace std;

int main() {
    double area, yield;

    cout << fixed << setprecision(2);

    try {
        cout << "Enter crop area in acres: ";
        if (!(cin >> area))
            throw invalid_argument("Area must be numeric.");

        cout << "Enter yield per acre: ";
        if (!(cin >> yield))
            throw invalid_argument("Yield must be numeric.");

        if (area <= 0)
            throw invalid_argument("Area must be positive.");

        if (yield < 0)
            throw invalid_argument("Yield cannot be negative.");

        cout << "Total Yield: "
             << area * yield << endl;
    }
    catch (const exception& e) {
        cout << "Error: " << e.what() << endl;
    }

    return 0;
}

COMPLETE SUBMISSION

#include <iostream>
#include <fstream>
#include <iomanip>
#include <string>
#include <vector>
#include <stdexcept>
#include <limits>
using namespace std;

// Question 1 and 3: Base class, constructors,
// static member and virtual destructor

class Crop {
protected:
    string name, season;
    double area, yieldPerAcre;
    static int cropCount;

public:
    Crop() : name("Unknown"), season("Unknown"),
             area(0), yieldPerAcre(0) {
        cropCount++;
    }

    Crop(string n, double a, string s, double y)
        : name(n), season(s), area(a), yieldPerAcre(y) {
        if (n.empty() || a <= 0 || y < 0)
            throw invalid_argument("Invalid crop details.");

        cropCount++;
    }

    virtual ~Crop() {
        cropCount--;
    }

    double getTotalYield() const {
        return area * yieldPerAcre;
    }

    string getName() const {
        return name;
    }

    static int getCropCount() {
        return cropCount;
    }

    // Question 5: Operator overloading
    bool operator>(const Crop& other) const {
        return getTotalYield() > other.getTotalYield();
    }

    // Question 4: Runtime polymorphism
    virtual void display() const {
        cout << "\nCrop Name: " << name
             << "\nArea: " << area << " acres"
             << "\nSeason: " << season
             << "\nYield per Acre: " << yieldPerAcre
             << "\nTotal Yield: " << getTotalYield()
             << endl;
    }

    // Question 7: Saving crop data
    virtual void save(ofstream& file) const = 0;
};

int Crop::cropCount = 0;

// Question 6: Abstract class

class Advisory {
public:
    virtual void giveAdvice() const = 0;
    virtual ~Advisory() {}
};

// Question 2: Derived cereal class

class CerealCrop : public Crop, public Advisory {
private:
    string fertilizerType;

public:
    CerealCrop(string n, double a, string s,
               double y, string f)
        : Crop(n, a, s, y), fertilizerType(f) {}

    void display() const override {
        cout << "\n--- Cereal Crop ---";
        Crop::display();
        cout << "Fertilizer: " << fertilizerType << endl;
    }

    void giveAdvice() const override {
        cout << "Advice: Maintain soil fertility and control weeds."
             << endl;
    }

    void save(ofstream& file) const override {
        file << "Cereal," << name << "," << area << ","
             << season << "," << yieldPerAcre << ","
             << fertilizerType << '\n';
    }
};

// Question 2: Derived vegetable class

class VegetableCrop : public Crop, public Advisory {
private:
    int irrigationFrequency;

public:
    VegetableCrop(string n, double a, string s,
                  double y, int i)
        : Crop(n, a, s, y), irrigationFrequency(i) {
        if (i <= 0)
            throw invalid_argument(
                "Irrigation frequency must be positive."
            );
    }

    void display() const override {
        cout << "\n--- Vegetable Crop ---";
        Crop::display();
        cout << "Irrigation Frequency: "
             << irrigationFrequency << " times/week"
             << endl;
    }

    void giveAdvice() const override {
        cout << "Advice: Check soil moisture and avoid overwatering."
             << endl;
    }

    void save(ofstream& file) const override {
        file << "Vegetable," << name << "," << area << ","
             << season << "," << yieldPerAcre << ","
             << irrigationFrequency << '\n';
    }
};

// Question 7: File handling

void saveCrops(const vector<Crop*>& crops) {
    ofstream file("crops.txt");

    if (!file)
        throw runtime_error("Unable to open file for writing.");

    for (const Crop* crop : crops)
        crop->save(file);

    if (!file)
        throw runtime_error("Error writing crop data.");

    cout << "Crop data saved successfully.\n";
}

void loadCrops() {
    ifstream file("crops.txt");

    if (!file) {
        cout << "No saved crop file found.\n";
        return;
    }

    string line;

    cout << "\n--- Saved Crop Records ---\n";

    while (getline(file, line))
        cout << line << endl;
}

// Main program

int main() {
    vector<Crop*> crops;
    int choice;

    cout << fixed << setprecision(2);

    do {
        cout << "\n===== SMART FARM MANAGEMENT =====\n";
        cout << "1. Add Cereal Crop\n";
        cout << "2. Add Vegetable Crop\n";
        cout << "3. Display Crops and Advice\n";
        cout << "4. Compare Crop Yields\n";
        cout << "5. Save Crop Data\n";
        cout << "6. Read Saved Data\n";
        cout << "7. Display Crop Count\n";
        cout << "8. Exit\n";
        cout << "Enter choice: ";

        if (!(cin >> choice)) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Please enter a valid number.\n";
            continue;
        }

        try {
            if (choice == 1 || choice == 2) {
                string name, season, fertilizer;
                double area, yield;
                int irrigation;

                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                cout << "Enter crop name: ";
                getline(cin, name);

                if (name.empty())
                    throw invalid_argument("Name cannot be empty.");

                cout << "Enter area in acres: ";
                if (!(cin >> area))
                    throw invalid_argument("Area must be numeric.");

                cout << "Enter season: ";
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                getline(cin, season);

                cout << "Enter yield per acre: ";
                if (!(cin >> yield))
                    throw invalid_argument("Yield must be numeric.");

                if (choice == 1) {
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');

                    cout << "Enter fertilizer type: ";
                    getline(cin, fertilizer);

                    crops.push_back(new CerealCrop(
                        name, area, season, yield, fertilizer
                    ));
                } else {
                    cout << "Enter irrigation frequency per week: ";

                    if (!(cin >> irrigation))
                        throw invalid_argument(
                            "Irrigation must be numeric."
                        );

                    crops.push_back(new VegetableCrop(
                        name, area, season, yield, irrigation
                    ));
                }

                cout << "Crop added successfully.\n";
            }
            else if (choice == 3) {
                if (crops.empty()) {
                    cout << "No crops available.\n";
                } else {
                    for (const Crop* crop : crops) {
                        crop->display();

                        const Advisory* advice =
                            dynamic_cast<const Advisory*>(crop);

                        if (advice)
                            advice->giveAdvice();
                    }
                }
            }
            else if (choice == 4) {
                if (crops.size() < 2) {
                    cout << "Add at least two crops first.\n";
                } else {
                    int a, b;

                    cout << "Enter first crop number (1-"
                         << crops.size() << "): ";
                    cin >> a;

                    cout << "Enter second crop number (1-"
                         << crops.size() << "): ";
                    cin >> b;

                    if (!cin || a < 1 || b < 1 ||
                        a > static_cast<int>(crops.size()) ||
                        b > static_cast<int>(crops.size())) {
                        cin.clear();
                        cin.ignore(
                            numeric_limits<streamsize>::max(), '\n'
                        );
                        throw out_of_range("Invalid crop number.");
                    }

                    Crop* first = crops[a - 1];
                    Crop* second = crops[b - 1];

                    if (*first > *second)
                        cout << first->getName()
                             << " has the higher total yield.\n";
                    else if (*second > *first)
                        cout << second->getName()
                             << " has the higher total yield.\n";
                    else
                        cout << "Both crops have equal yields.\n";
                }
            }
            else if (choice == 5) {
                saveCrops(crops);
            }
            else if (choice == 6) {
                loadCrops();
            }
            else if (choice == 7) {
                cout << "Current crop count: "
                     << Crop::getCropCount() << endl;
            }
            else if (choice == 8) {
                cout << "Exiting program.\n";
            }
            else {
                cout << "Invalid choice. Try again.\n";
            }
        }
        catch (const exception& e) {
            cout << "Error: " << e.what() << endl;

            if (cin.fail()) {
                cin.clear();
                cin.ignore(
                    numeric_limits<streamsize>::max(), '\n'
                );
            }
        }

    } while (choice != 8);

    try {
        saveCrops(crops);
    }
    catch (const exception& e) {
        cout << "Unable to save data: " << e.what() << endl;
    }

    for (Crop* crop : crops)
        delete crop;

    crops.clear();

    return 0;
}
OUTPUT
--- Cereal Crop ---
Crop Name: Wheat
Area: 5.00 acres
Season: Winter
Yield per Acre: 20.00
Total Yield: 100.00
Fertilizer: Urea
Advice: Maintain soil fertility and control weeds.
--- Cereal Crop ---
Crop Name: Rice
Area: 4.00 acres
Season: Summer
Yield per Acre: 25.00
Total Yield: 100.00
Fertilizer: DAP
Advice: Maintain soil fertility and control weeds.

--- Vegetable Crop ---
Crop Name: Tomato
Area: 3.00 acres
Season: Summer
Yield per Acre: 15.00
Total Yield: 45.00
Irrigation Frequency: 4 times/week
Advice: Check soil moisture and avoid overwatering.

Both crops have equal yields.

Crop data saved successfully.
Current crop count: 3
