# lab7
//1.
#include <iostream>

float operator "" _Kelvin(const char* x)
{
	float value = strtof(x, nullptr);
	return value - 273.15;
}

float operator "" _Fahrenheit(const char* x)
{
	float value = strtof(x, nullptr);
	return (value - 32) / 1.8;
}

int main()
{
	float a = 300_Kelvin;
	float c = 25.5_Kelvin;
	float b = 120_Fahrenheit;
	std::cout << a << ' ' << b << ' ' << c;
	return 0;
}
//2.
#ifndef VECTOR_H
#define VECTOR_H

#include <iostream>
#include <string>

template <typename T>
class Vector {

	int count;
	int currentPosition;
	T* vector;
	T empty;
	const T* empty_C = nullptr;

	void Debug(std::string message)
	{
		std::cout << message << std::endl;
	}

	void Resize()
	{
		T* aux = new T[count * 2];
		count *= 2;
		for (int i = 0; i < currentPosition; i++)
			aux[i] = vector[i];
		delete[] vector;
		vector = aux;
	}


	Vector(int length)
	{
		if (length < 1)
		{
			Debug("Error! The length of the vector can't be below 1.");
			return;
		}
		count = length;
		currentPosition = 0;
		vector = new T[length];
	}

	void Push(T obj)
	{
		if (currentPosition < count)
		{
			vector[currentPosition++] = obj;
			return;
		}
		Resize();

		vector[currentPosition++] = obj;
	}

	T Pop()
	{
		if (currentPosition > 0)
			return vector[currentPosition];
		Debug("Error! The vector is empty!");
		return empty;
	}

	void Remove(int index)
	{
		if (index < 0 || index > currentPosition)
		{
			Debug("Error! Can't remove at that index.");
			return;
		}
		for (int i = index; i < currentPosition - 1; i++)
			vector[i] = vector[i + 1];
		currentPosition--;
	}

	void Insert(T obj, int index)
	{
		if (index < 0 || index > currentPosition)
		{
			Debug("Error! Can't insert element at the specified index.");
			return;
		}

		if (currentPosition == index || currentPosition >= count)
		{
			Resize();
			for (int i = currentPosition; i > index; i--)
				vector[i] = vector[i - 1];
			vector[index] = obj;
			currentPosition++;
			return;
		}

		// This means that we have space left in the vector.
		for (int i = currentPosition; i >= index; i--)
			vector[i] = vector[i - 1];
		vector[index] = obj;
		currentPosition++;
	}

	void Sort(bool IsGreater(T, T))
	{
		if (IsGreater == nullptr)
		{
			T aux;
			bool ok;
			do
			{
				ok = true;
				for (int i = 0; i < currentPosition - 1; i++)
					if (vector[i] < vector[i + 1])
					{
						aux = vector[i];
						vector[i] = vector[i + 1];
						vector[i + 1] = aux;
						ok = false;
					}
			} while (!ok);
			return;
		}
		T aux;
		bool ok;
		do
		{
			ok = true;
			for (int i = 0; i < currentPosition - 1; i++)
				if (IsGreater(vector[i], vector[i + 1]) == true)
				{
					aux = vector[i];
					vector[i] = vector[i + 1];
					vector[i + 1] = aux;
					ok = false;
				}
		} while (!ok);

	}

	const T& Get(int index)
	{
		if (index < 0 || index >= currentPosition)
		{
			Debug("Error! Can't get element at the specified index.");
			return *empty_C;
		}
		return vector[index];
	}

	void Set(T obj, int index)
	{
		if (index < 0 || index >= currentPosition)
		{
			Debug("Error! Can't set element at the specified index.");
			return;
		}
		vector[index] = obj;
	}

	int Count() const 
	{
		return currentPosition;
	}

	int firstIndexOf(T obj, bool IsEqual(T, T))
	{
		if (IsEqual == nullptr)
		{
			for (int i = 0; i < currentPosition; i++)
				if (obj == vector[i])
					return i;
			return -1;
		}
		for (int i = 0; i < currentPosition; i++)
			if (IsEqual(vector[i], obj) == true)
				return i;
		return -1;
	}
};


template <>
class Vector <const char*> {

		int count;
		int currentPosition;
		char** vector;
		char* empty;

		void Debug(std::string message)
		{
			std::cout << message << std::endl;
		}

		void Resize()
		{
			char** aux = new char*[count * 2];
	   	        count *= 2;
			for (int i = 0; i < currentPosition; i++)
				aux[i] = vector[i];
			delete[] vector;
   vector = aux;
		}


	Vector(int length)
	{
		if (length < 1)
		{
			Debug("Error! The length of the vector can't be below 1.");
			return;
		}
		count = length;
		currentPosition = 0;
		vector = new char*[length];
	}

	void Push(char* obj)
	{
		if (currentPosition < count)
		{
			vector[currentPosition++] = obj;
         		return;
		}
		Resize();

		vector[currentPosition++] = obj;
	}

	char* Pop()
	{
		if (currentPosition > 0)
			return vector[currentPosition];
		Debug("Error! The vector is empty!");
		return empty;
	}

	void Remove(int index)
	{
		if (index < 0 || index > currentPosition)
		{
			Debug("Error! Can't remove at that index.");
			return;
		}
		for (int i = index; i < currentPosition - 1; i++)
			vector[i] = vector[i + 1];
		currentPosition--;
	}

	void Insert(char* obj, int index)
	{
		if (index < 0 || index > currentPosition)
		{
			Debug("Error! Can't insert element at the specified index.");
			return;
		}

		if (currentPosition == index)
		{
			Resize();
			for (int i = currentPosition; i >= index; i--)
				vector[i + 1] = vector[i];
			vector[index] = obj;
			return;
   }

		// This means that we have space left in the vector.
		for (int i = currentPosition; i >= index; i--)
			vector[i + 1] = vector[i];
		vector[index] = obj;
	}

	void Sort(bool IsGreater(char*, char*))
	{
		if (IsGreater == nullptr)
		{
			char* aux;
			bool ok;
			do
			{
				ok = true;
				for (int i = 0; i < currentPosition - 1; i++)
					if (vector[i] < vector[i + 1])
					{
						aux = vector[i];
						vector[i] = vector[i + 1];
						vector[i + 1] = aux;
						ok = false;
					}
			} while (!ok);
			return;
		}
		char* aux;
		bool ok;
  	        do
		{
			ok = true;
			for (int i = 0; i < currentPosition - 1; i++)
				if (IsGreater(vector[i], vector[i + 1]) == true)
				{
					aux = vector[i];
					vector[i] = vector[i + 1];
					vector[i + 1] = aux;
					ok = false;
				}
		} while (!ok);

	}

        char*& Get(int index)
	{
		if (index < 0 || index >= currentPosition)
		{
			Debug("Error! Can't get element at the specified index.");
			return empty;
		}
		return vector[index];
        }

	void Set(char* obj, int index)
	{
          if (index < 0 || index >= currentPosition)
		{
			Debug("Error! Can't set element at the specified index.");
			return;
		}
		vector[index] = obj;
	}

	int Count() const
	{
		return currentPosition;
	}

	int firstIndexOf(T obj, bool IsEqual(T, T))
        {
		if (IsEqual == nullptr)
		{
			for (int i = 0; i < currentPosition; i++)
				if (obj == vector[i])
					return i;
			return -1;
		}
		for (int i = 0; i < currentPosition; i++)
			if (IsEqual(vector[i], obj) == true)
				return i;
		return -1;
	}
};

#endif
