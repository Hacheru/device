#include <iostream>
#include <stdexcept>
#include <ctime>
//#include <vector>
#include <array>

class pyramid
{
public:
    pyramid() {
        A = 0;
        H = 0;
    }
    pyramid(int a, int b)
    {
        A = a;
        H = b;
    }
    double EvalVolume()
    {
        return (1 / 3) * H * A * A;
    }
    void getA_H(int a, int b)
    {
        A = a;
        H = b;
    }

private:
    int A, H;
};

template <typename T>
class Vector {
public:
    Vector() : size_(0), capacity_(1), data_(new T[1]) {}

    ~Vector() {
        delete[] data_;
    }

    void push_back(const T& value) {
        if (size_ == capacity_) {
            resize(capacity_ * 2);
        }
        data_[size_++] = value;
    }

    void insert(int i, const T& value) {
        if (i < 0 || i > size_) {
            throw std::out_of_range("Index out of range");
        }
        if (size_ == capacity_) {
            resize(capacity_ * 2);
        }
        for (int j = size_; j > i; --j) {
            data_[j] = data_[j - 1];
        }
        data_[i] = value;
        ++size_;
    }

    void remove(int i) {
        if (i < 0 || i >= size_) {
            throw std::out_of_range(" Index out of range");
        }
        for (int j = i; j < size_ - 1; ++j) {
            data_[j] = data_[j + 1];
        }
        --size_;
        if (size_ > 0 && size_ == capacity_ / 4) {
            resize(capacity_ / 2);
        }
    }

    T& operator[](int i) {
        if (i < 0 || i >= size_) {
            throw std::out_of_range("Index out of range");
        }
        return data_[i];
    }

    int size() const {
        return size_;
    }

private:
    void resize(int new_capacity) {
        T* new_data = new T[new_capacity];
        for (int i = 0; i < size_; ++i) {
            new_data[i] = data_[i];
        }
        delete[] data_;
        data_ = new_data;
        capacity_ = new_capacity;
    }

    int size_;
    int capacity_;
    T* data_;
};
namespace myspace {
    template <typename T>
    class vector {
    public:
        vector() : data(nullptr), N(0) {}
        vector(int _N, T _Val);
        ~vector();
        T operator[] (int i);
        void resize(int newSize, T defaultValue);
        int size() {
            return N;
        }
        void insert(int index, T value);
        void remove(int index);

    protected:
        T** data;
        int N;
    };

    template <typename T>
    vector<T>::vector(int _N, T _Val) : N(_N) {
        data = new T * [N];
        for (int i = 0; i < N; i++) {
            data[i] = new T(_Val);
        }
    }

    template <typename T>
    vector<T>::~vector() {
        for (int i = 0; i < N; i++) {
            delete data[i];
            std::cout << i << "\n";
        }
        delete[] data;
    }

    template <typename T>
    T vector<T>::operator[] (int i) {
        try {
            if (i >= 0 && i < N) {
                return *data[i];
            }
            else {
                throw(i);
            }
        }
        catch (int i) {
            std::cout << "out of range, try to access " << i << " out of " << N << "\n";
            exit(-1);
        }
    }

    template <typename T>
    void vector<T>::insert(int index, T value) {
        if (index < 0 || index > N) {
            std::cout << "Index out of range\n";
            return;
        }
        T** newData = new T * [N + 1];
        for (int i = 0, j = 0; i < N + 1; ++i) {
            if (i == index) {
                newData[i] = new T(value);
            }
            else {
                newData[i] = data[j++];
            }
        }
        delete[] data;
        data = newData;
        ++N;
    }

    template <typename T>
    void vector<T>::remove(int index) {
        if (index < 0 || index >= N) {
            std::cout << "Index out of range\n";
            return;
        }
        delete data[index];
        T** newData = new T * [N - 1];
        for (int i = 0, j = 0; i < N; ++i) {
            if (i != index) {
                newData[j++] = data[i];
            }
        }
        delete[] data;
        data = newData;
        --N;
    }

    template <typename T>
    void vector<T>::resize(int newSize, T defaultValue) {
        T** newData = new T * [newSize];
        for (int i = 0; i < newSize; ++i) {
            if (i < N) {
                newData[i] = data[i];
            }
            else {
                newData[i] = new T(defaultValue);
            }
        }
        delete[] data;
        data = newData;
        N = newSize;
    }
}

using namespace std;

int main() {
    Vector<pyramid> my_vector;
    myspace:: vector <pyramid> vec;
    pyramid p1(10, 20);
    pyramid p2(30, 40);
    pyramid p3(50, 60);
    //my_vector.remove(0);
    clock_t start_insert = std::clock();
    for (int i = 0; i < 2000; i++) {
        my_vector.insert(0, p1);
    }
    clock_t end_insert = std::clock();
    clock_t start_remove = std::clock();
    for (int i = 0; i < 1000; i++) {
        my_vector.remove(0);
    }
    clock_t end_remove = std::clock();
    clock_t start_insert1  = std::clock();
    for (int i = 0; i < 200; i++) {
        my_vector.insert(1, p2);
    }
    clock_t end_insert1  = std::clock();
    clock_t start_remove1  = std::clock();
    for (int i = 0; i < 100; i++) {
        my_vector.remove(0);
    }
    clock_t end_remove1  = std::clock();
    clock_t start_insert2 = std::clock();
    for (int i = 0; i < 200; i++) {
        my_vector.insert(0, p3);
    }
    clock_t end_insert2 = std::clock();
    clock_t start_remove2  = std::clock();
    for (int i = 0; i < 100; i++) {
        my_vector.remove(0);
    }
    clock_t end_remove2  = std::clock();
    clock_t start_insert3 = std::clock();
    for (int i = 0; i < 200; i++) {
        vec.insert(0, p1);
    }
    clock_t end_insert3 = std::clock();
    clock_t start_remove3 = std::clock();
    for (int i = 0; i < 100; i++) {
        vec.remove(0);
    }
    clock_t end_remove3 = std::clock();
    clock_t start_insert4 = std::clock();
    for (int i = 0; i < 2000; i++) {
        vec.insert(1, p2);
    }
    clock_t end_insert4 = std::clock();
    clock_t start_remove4 = std::clock();
    for (int i = 0; i < 1000; i++) {
        vec.remove(0);
    }
    clock_t end_remove4 = std::clock();
    clock_t start_insert5 = std::clock();
    for (int i = 0; i < 200; i++) {
        vec.insert(2, p2);
    }
    clock_t end_insert5 = std::clock();
    clock_t start_remove5 = std::clock();
    for (int i = 0; i < 100; i++) {
        vec.remove(0);
    }
    clock_t end_remove5 = std::clock();
    double insert_duration = double(end_insert - start_insert) / CLOCKS_PER_SEC;
    std::cout << "Insert duration1: " << insert_duration << " seconds" << std::endl;
    double remove_duration = double(end_remove - start_remove) / CLOCKS_PER_SEC;
    std::cout << "Remove duration1: " << remove_duration  << " seconds" << std::endl;

    std::cout << "********************************************************************"<<endl;
    double insert_duration1  = double(end_insert1 - start_insert1 ) / CLOCKS_PER_SEC;
    std::cout << "Insert duration2: " << insert_duration1  << " seconds" << std::endl;
    double remove_duration1  = double(end_remove1  - start_remove1 ) / CLOCKS_PER_SEC;
    std::cout << "Remove duration2: " << remove_duration1 << " seconds" << std::endl;


    std::cout << "********************************************************************" << endl;
    double insert_duration2 = double(end_insert2 - start_insert2) / CLOCKS_PER_SEC;
    std::cout << "Insert duration3: " << insert_duration2 << " seconds" << std::endl;
    double remove_duration2 = double(end_remove2 - start_remove2) / CLOCKS_PER_SEC;
    std::cout << "Remove duration3: " << remove_duration2 << " seconds" << std::endl;
    std::cout << "********************************************************************" << endl;

    double insert_duration3 = double(end_insert3 - start_insert3) / CLOCKS_PER_SEC;
    std::cout << "Insert duration4: " << insert_duration3 << " seconds" << std::endl;
    double remove_duration3 = double(end_remove3 - start_remove3) / CLOCKS_PER_SEC;
    std::cout << "Remove duration4: " << remove_duration3  << " seconds" << std::endl;
    std::cout << "********************************************************************" << endl;
    double insert_duration4 = double(end_insert4 - start_insert4) / CLOCKS_PER_SEC;
    std::cout << "Insert duration5: " << insert_duration << " seconds" << std::endl;
    double remove_duration4 = double(end_remove4 - start_remove4) / CLOCKS_PER_SEC;
    std::cout << "Remove duration5: " << remove_duration1 << " seconds" << std::endl;


    std::cout << "********************************************************************" << endl;

    double insert_duration5 = double(end_insert5 - start_insert5) / CLOCKS_PER_SEC;
    std::cout << "Insert duration6: " << insert_duration1 << " seconds" << std::endl;
    double remove_duration5 = double(end_remove5 - start_remove5) / CLOCKS_PER_SEC;
    std::cout << "Remove duration6: " << remove_duration1 << " seconds" << std::endl;


    std::cout << "********************************************************************" << endl;
    std::cout << "vectorpointer is faster than vector";
    // Access and print the pyramid objects
    /*for (int i = 0; i < my_vector.size(); ++i) {
        pyramid& p = my_vector[i];
        std::cout << "Pyramid " << i << ": A = " << p.EvalVolume() << std::endl;
    }*/
    return 0;
}
