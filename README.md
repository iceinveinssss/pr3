# pr3
1.
#include <iostream>
#include <fstream>

int main() 
{
    setlocale(0, "");
    std::ifstream inputFile("D:/a.txt"); 
    double number;
    int positiveCount = 0; 
    double positiveSum = 0.0; 

    if (!inputFile.is_open()) 
    {
        std::cerr << "Ошибка при открытии файла." << std::endl;
        return 1; 
    }

    while (inputFile >> number)
    { 
        if (number > 0) 
        {
            positiveCount++; 
            positiveSum += number; 
        }
    }

    inputFile.close(); 

    std::cout << "Количество положительных элементов: " << positiveCount << std::endl;
    std::cout << "Сумма положительных элементов: " << positiveSum << std::endl;

    return 0; 
}

2.
#include <iostream>
#include <fstream>

int main() 
{
    setlocale(0, "");
    std::ifstream inputFile1("D:/a1.txt"); 
    std::ifstream inputFile2("D:/a2.txt"); 
    double number;
    int zeroCount = 0; 

    if (!inputFile1.is_open() || !inputFile2.is_open()) 
    {
        std::cerr << "Ошибка при открытии файла." << std::endl;
        return 1; 
    }

  
    while (inputFile1 >> number) 
    {
        if (number == 0) 
        {
            zeroCount++; 
        }
    }


    while (inputFile2 >> number) 
    {
        if (number == 0)
        {
            zeroCount++;
        }
    }

    inputFile1.close(); 
    inputFile2.close(); 

    std::cout << "Количество нулевых элементов в двух файлах: " << zeroCount << std::endl;

    return 0; 
}

3.
#include <iostream>
#include <fstream>
#include <cctype>

int main()
{
    setlocale(0, "");
    std::ifstream inputFile("D:/original.txt"); 
    std::ofstream outputFile("D:/modified.txt"); 

    if (!inputFile.is_open())
    {
        std::cerr << "Ошибка при открытии исходного файла." << std::endl;
        return 1;
    }

    if (!outputFile.is_open())
    {
        std::cerr << "Ошибка при создании нового файла." << std::endl;
        return 1;
    }

    char ch;
    while (inputFile.get(ch))
    { 
        if (isdigit(ch)) 
        { 
            outputFile.put('*'); 
        }
        else
        {
            outputFile.put(ch); 
        }
    }

    inputFile.close(); 
    outputFile.close(); 

    std::cout << "Цифры в файле были успешно";
    return 0; 
}

4.
#include <iostream>
#include <fstream>

int main() 
{
    setlocale(0, "");
    std::ifstream inputFile("D:/numbers.txt"); 
    if (!inputFile.is_open()) 
    {
        std::cerr << "Ошибка при открытии файла." << std::endl;
        return 1; 
    }

    int number;
    int sum = 0; 
    int count = 0; 
    int index = 1; 

    while (inputFile >> number) 
    { 
        if (index % 3 == 0 && number > 0) 
        { 
            sum += number;
            count++;
        }
        index++;
    }

    inputFile.close(); 

    if (count > 0) 
    {
        double average = static_cast<double>(sum) / count; 
        std::cout << "Среднее значение среди положительных чисел, номера которых кратны трем: " << average << std::endl;
    }
    else {
        std::cout << "Положительные числа, номера которых кратны трем, в файле отсутствуют." << std::endl;
    }

    return 0; 
}

5.
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <sstream>

struct CarOwner 
{
    std::string surname, name, patronymic, phone, address, carBrand, carNumber, passportNumber;
};

std::vector<CarOwner> readCarOwners(const std::string& filename) 
{
    std::vector<CarOwner> owners;
    std::ifstream file(filename);
    std::string line;

    while (std::getline(file, line)) {
        std::stringstream ss(line);
        CarOwner owner;
        std::getline(ss, owner.surname, ';');
        std::getline(ss, owner.name, ';');
        std::getline(ss, owner.patronymic, ';');
        std::getline(ss, owner.phone, ';');
        std::getline(ss, owner.address, ';');
        std::getline(ss, owner.carBrand, ';');
        std::getline(ss, owner.carNumber, ';');
        std::getline(ss, owner.passportNumber, ';');
        owners.push_back(owner);
    }

    file.close();
    return owners;
}

void saveVazOwners(const std::vector<CarOwner>& owners, const std::string& filename) 
{
    std::ofstream file(filename);
    for (const auto& owner : owners) 
    {
        if (owner.carBrand == "ВАЗ") 
        {
            file << owner.surname << ";" << owner.name << ";" << owner.patronymic << ";"
                << owner.phone << ";" << owner.address << ";" << owner.carBrand << ";"
                << owner.carNumber << ";" << owner.passportNumber << "\n";
        }
    }
    file.close();
}

int main() 
{
    setlocale(0, "");

    const std::string inputFilename = "D:/car_owners.txt";
    const std::string outputFilename = "D:/vaz_owners.txt";

    auto owners = readCarOwners(inputFilename);
    saveVazOwners(owners, outputFilename);

    std::cout << "Данные о владельцах автомобилей марки ВАЗ сохранены в файл: " << outputFilename << std::endl;
    return 0;
}

8.
#include <iostream>
#include <fstream>
#include <vector>
#include <string>

struct Monitor
{
    std::string firmName;
    float sizeInInches;
    float price;

    
    void serialize(std::ofstream& out) const 
    {
        size_t nameLength = firmName.length();
        out.write(reinterpret_cast<const char*>(&nameLength), sizeof(nameLength));
        out.write(firmName.data(), nameLength);
        out.write(reinterpret_cast<const char*>(&sizeInInches), sizeof(sizeInInches));
        out.write(reinterpret_cast<const char*>(&price), sizeof(price));
    }

    static Monitor deserialize(std::ifstream& in) 
    {
        Monitor m;
        size_t nameLength;
        in.read(reinterpret_cast<char*>(&nameLength), sizeof(nameLength));
        m.firmName.resize(nameLength);
        in.read(&m.firmName[0], nameLength);
        in.read(reinterpret_cast<char*>(&m.sizeInInches), sizeof(m.sizeInInches));
        in.read(reinterpret_cast<char*>(&m.price), sizeof(m.price));
        return m;
    }
};

int main() 
{
    setlocale(0, "");
    std::vector<Monitor> monitors = 
    {
        {"Samsung", 24.0, 150.0},
        {"LG", 27.0, 200.0},
        {"Dell", 19.0, 120.0},
        {"Acer", 18.5, 100.0}
    };


    std::ofstream outFile("D:/monitors.bin", std::ios::binary);
    for (const auto& monitor : monitors) 
    {
        monitor.serialize(outFile);
    }
    outFile.close();

 
    std::vector<Monitor> loadedMonitors;
    std::ifstream inFile("D:/monitors.bin", std::ios::binary);
    while (inFile.peek() != EOF) 
    {
        loadedMonitors.push_back(Monitor::deserialize(inFile));
    }
    inFile.close();

 
    float totalPrice = 0;
    int count = 0;
    for (const auto& monitor : loadedMonitors) 
    {
        if (monitor.sizeInInches >= 19) 
        {
            totalPrice += monitor.price;
            count++;
        }
    }

    float averagePrice = (count > 0) ? totalPrice / count : 0;
    std::cout << "Средняя цена мониторов с диагональю не менее 19 дюймов: " << averagePrice << std::endl;

    for (const auto& monitor : loadedMonitors) 
    {
        if (monitor.sizeInInches >= 19) 
        {
            std::cout << "Фирма: " << monitor.firmName << ", Размер: " << monitor.sizeInInches << "\", Стоимость: $" << monitor.price << std::endl;
        }
    }

    return 0;
}

9.
#include <iostream>
#include <fstream>
#include <string>

using namespace std;


void writeToFile(const string& text, const string& filename) 
{
    ofstream file(filename, ios::binary);
    if (!file.is_open()) 
    {
        cout << "Ошибка открытия файла для записи." << endl;
        return;
    }
    file.write(text.c_str(), text.size());
    file.close();
}

void rewriteFile(const string& filename) 
{
    ifstream file(filename, ios::binary);
    if (!file.is_open()) 
    {
        cout << "Ошибка открытия файла для чтения." << endl;
        return;
    }

    file.seekg(0, ios::end);
    size_t fileSize = file.tellg();
    file.seekg(0, ios::beg);

    char* buffer = new char[fileSize];
    file.read(buffer, fileSize);

    for (size_t i = 0; i < fileSize; ++i) 
    {
        if (buffer[i] >= 'а' && buffer[i] <= 'я') 
        {
            buffer[i] -= ('а' - 'А');
        }
    }

    file.close();

    ofstream rewriteFile(filename, ios::binary);
    rewriteFile.write(buffer, fileSize);
    rewriteFile.close();

    delete[] buffer;
}

void printFile(const string& filename) 
{
    ifstream file(filename, ios::binary);
    if (!file.is_open()) 
    {
        cout << "Ошибка открытия файла для чтения." << endl;
        return;
    }

    char ch;
    cout << "Содержимое файла:" << endl;
    while (file.get(ch)) 
    {
        cout << ch;
    }
    cout << endl;

    file.close();
}

int main()
{
    setlocale(0, "");
    string text = "пример текста для записи в файл";
    string filename = "D:/dat.bin";

    writeToFile(text, filename);

    cout << "Исходное содержимое файла:" << endl;
    printFile(filename);

    rewriteFile(filename);

    cout << "Содержимое файла после изменений:" << endl;
    printFile(filename);

    return 0;
}

10.
#include <iostream>
#include <fstream>
#include <vector>
#include <cstring>

struct Purchase
{
    char buyer[50]; 
    char date[12]; 
    double cost_first_half; 
    double cost_second_half; 
    double discount; 
};

void createAndWriteToFile(const char* filename)
{
    std::ofstream out(filename, std::ios::binary);
    if (!out.is_open()) 
    {
        std::cerr << "Не удалось открыть файл для записи." << std::endl;
        return;
    }

    Purchase purchases[] =
    {
        {"Иванов И.И.", "01.01.2023", 12000, 15000, 10},
        {"Петров П .П.", "12.03.2023", 8000, 9500, 5},
        {"Сидоров С.С.", "25.06.2023", 11000, 10500, 8}
    };

    for (const auto& purchase : purchases) 
    {
        out.write(reinterpret_cast<const char*>(&purchase), sizeof(Purchase));
    }
    out.close();
}

void updateDiscounts(const char* filename)
{
    std::fstream file(filename, std::ios::binary | std::ios::in | std::ios::out);
    if (!file.is_open()) 
    {
        std::cerr << "Не удалось открыть файл для чтения и записи." << std::endl;
        return;
    }

    Purchase purchase;
    std::vector<Purchase> updatedPurchases;

    while (file.read(reinterpret_cast<char*>(&purchase), sizeof(Purchase)))
    {
        if (purchase.cost_first_half >= 10000 && purchase.cost_second_half >= 10000)
        {
            purchase.discount += 7; 
        }
        updatedPurchases.push_back(purchase);
    }

    file.clear(); 
    file.seekp(0, std::ios::beg); 

    for (const auto& updatedPurchase : updatedPurchases)
    {
        file.write(reinterpret_cast<const char*>(&updatedPurchase), sizeof(Purchase));
    }

    file.close();
}

int main() 
{
    setlocale(0, "");
    const char* filename = "D:/purchases.bin";
    createAndWriteToFile(filename);
    updateDiscounts(filename);

    std::cout << "Обработка файла завершена." << std::endl;
    return 0;
}
