

# Data Science Practicals (5 to 9)

## Practical 05

### Aim 5A: Perform error management on the given data using pandas package
```python
import pandas as pd
data = {
"Name": ["John", "Alice", None, "John"],
"Age" : [23, None, 25, 24],
"Salary" : [50000, 60000, None, 50000]
}
df = pd.DataFrame(data)
print("ORIGINAL DATA")
print(data)
#Removing Duplicate Data
df = df.drop_duplicates()
#Filling Missing Details
df["Age"] = df["Age"].fillna(df["Age"].mean())
df["Salary"] = df["Salary"].fillna(df["Salary"].mean())
df["Name"] = df["Name"].fillna("Unknown")
print("Cleaned Data")
print(df)
```

### Aim 5B: Write python program to create the network routing diagram from the given data on routers
```python
import networkx as nx
import matplotlib.pyplot as plt
G = nx.Graph()
G.add_edges_from([(1,2),
(1,3),
(2,4),
(3,5)
])
print("Is DAG?", nx.is_directed_acyclic_graph(G))
nx.draw(G, with_labels=True,
node_color="Lightblue",
node_size=2000)
plt.show()
```

### Aim 5C: Write a Python program to build acyclic graph
```python
import networkx as nx
import matplotlib.pyplot as plt
G = nx.DiGraph()
G.add_edges_from([
("R1", "R2"), ("R1", "R3"), ("R3","R5"), ("R2", "R4")])
nx.draw(G, with_labels=True,
node_color="Blue",
node_size=2000)
plt.show()
```

### Aim 5D: Write Python Program Python program to pick the content for billboards from the given data
```python
import pandas as pd
df = pd.DataFrame({
"Product":["TV", "Phone", "Laptop", "Tablet"],
"Sales": [120,300,200,150]
})
billboard = df.sort_values("Sales", ascending=False).head(2)
print(billboard)
```

### Aim 5E: Write a python program to generate GML File using CSV file
```python
import pandas as pd
import networkx as nx

data = {
    "Source": ["A", "B", "C"],
    "Target": ["B", "C", "A"]
}

df = pd.DataFrame(data)
G = nx.from_pandas_edgelist(df, source="Source", target="Target")
nx.write_gml(G, "network.gml")
print("... GML FILE CREATED")
```

### Aim 5F: Write a python program to plan location of warehouse from the given data
```python
import pandas as pd
df = pd.DataFrame ({
"X": [10,20,30,40],
"Y": [15,25,35,45]
})
warehouse_x = df["X"].mean()
warehouse_y = df["Y"].mean()
print("Suggested Warehouse Location")
print (warehouse_x, warehouse_y)
```

### Aim 5G: Write python program using data science via clustering to determine new warehouse location using given data
```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans

df = pd.DataFrame({
"X":[2,3,4,20,21,22],
"Y":[2,4,3,20,22,21]
})

kmeans = KMeans(n_clusters=3, random_state=0)
df["Cluster"] = kmeans.fit_predict(df)
print(df)

plt.scatter(df["X"],df["Y"],
c=df["Cluster"],
s=100)

plt.scatter(kmeans.cluster_centers_[:,0],
kmeans.cluster_centers_[:,1],
marker = "X",
s=200)

plt.show()
```

### Aim 5H: Using the given data write python program to plan the shipping routers from best-fit international logistics
```python
import networkx as nx
G = nx.Graph()
G.add_weighted_edges_from([
("Mumbai", "Delhi",5),
("Delhi", "Chennai",7),
("Mumbai", "Chennai", 15)
])
route = nx.shortest_path(G, "Mumbai", "Chennai", weight="weight")
print(route)
```

### Aim 5I: Write python program to delete the best packing option to shipping container from the given data
```python
import pandas as pd
df = pd.DataFrame({
"Packing": ["Box A", "Box B", "Box C"],
"Cost": [500,300,700]
})
best = df["Cost"].idxmin()
df= df.drop(best)
print(df)
```

### Aim 5J: Write a python program to create delivery route using the given data
```python
import networkx as nx
import matplotlib.pyplot as plt
G = nx.DiGraph()
G.add_edges_from([
("Warehouse", "Area1"),
("Area1", "Area2"),
("Area2", "Customer")
])
nx.draw(G,
with_labels=True,
arrows=True)
plt.show()
```

### Aim 5K: Write a python program to create simple forex trading planner
```python
import pandas as pd
df=pd.DataFrame({
"Rate": [82.5,83.0,82.8,84.1]
})
buy = df["Rate"].min()
sell=df["Rate"].max()
print("Buy AT:", buy)
print("Sell AT:", sell)
profit = sell - buy
print("Profit:", profit)
```

### Aim 5L: Write python program to process the balance sheet to ensure the only good data is processing
```python
import pandas as pd
df = pd.DataFrame({
"Revenue": [100000, None, 120000],
"Expense": [60000, 50000, None]
})
df = df.fillna(0)
df["Profit"] = df ["Revenue"]-df["Expense"]
print(df)
```

### Aim 5M: Write python program to generate payroll from the given data
```python
import pandas as pd
df = pd.DataFrame({
"Employee":["John", "Alice", "Bob"], 
"Basic": [30000, 40000, 35000]
})
df["HRA"] = df["Basic"]*0.20
df["DA"] = df ["Basic"]*0.10
df["Gross Salary"] =(df["Basic"]+df["HRA"]+df["DA"])
print(df)
```

## Practical 06

### Aim 6: Build the Time Hub, Links and Satellites
```python
import pandas as pd
import matplotlib.pyplot as plt
import networkx as nx

data = {
"CustomerID": [101, 102, 101, 102, 104, 102],
"OrderID": [5001,5002,5003, 5004,5005,5006],
"CustomerName": ["Yajna", "Vishal", "Kalpana", "Rakesh", "Troy", "Hydra"],
"Location":["Kenya", "Brooklyn", "India", "America", "Chicago", "Paris"],
"Amount": [2500, 1500, 1000, 3000, 2500,6000],
"LoadDate":pd.Timestamp.today().date()
}
df = pd.DataFrame(data)

#Hub Table
hub_customer = df[["CustomerID"]].drop_duplicates()

#Link Table
link_customer = df[["CustomerID", "OrderID"]].drop_duplicates()

#Satellite Table
satellite_customer = df[["CustomerID", "OrderID", "CustomerName", "Location"]]

#Visualize The Data
orders = df.groupby("CustomerName") ["OrderID"].count()
plt.figure(figsize=(7, 4))
orders.plot(kind="bar", color="orange")
plt.xlabel("CustomerName")
plt.ylabel("OrderID")
plt.xticks(rotation= 0)
plt.show()

G = nx.Graph()
for customer in hub_customer["CustomerID"]:
    G.add_node(customer, node_type="Customer")
for _, row in link_customer.iterrows():
    customer_id = row["CustomerID"]
    order_id = row["OrderID"]
    G.add_node(order_id, node_type="Order")
    G.add_edge(customer_id, order_id)
    
plt.figure(figsize=(10,6))
pos = nx.spring_layout(G, seed=42)

nx.draw(
G,
pos,
with_labels = True,
node_size=600,
font_size=9
)
plt.title("Customer Order Relationship")
plt.show()
```

## Practical 07

### Aim 7: Transforming Data
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("/content/titanic.csv")

df.head()
df.tail()
df.describe()

df.info()
df.shape

#Remove duplicates
df = df.drop_duplicates()

print("\n Missing Values")
print(df.isnull().sum())

# Fill Missing Values
df['Age'].fillna (df['Age'].mean(), inplace=True)
df['Embarked'].fillna(df['Embarked'].mode()[0], inplace=True)

df['AgeGroup'] = pd.cut(
    df['Age'], 
    bins=[0, 12, 18, 35, 60, 100],
    labels=["Child", "Teen", "Adult", "Middle Age", "Senior"]
)

#Create Family Size
df["FamilySize"] = df["SibSp"]+df["Parch"]

df["IsAlone"] = np.where(df["FamilySize"] == 1, "Yes", "No")

# Fare Category
df['FareCategory'] = pd.cut(
    df['Fare'],
    bins=[0, 100, 200, 300, 520],
    labels=["Low", "Medium", "High", "Very High"]
)

survival = df.groupby("Pclass")["Survived"].mean() * 100
plt.figure(figsize=(8, 6))
survival.plot(kind="bar")
plt.xlabel("Passenger Class")
plt.ylabel("Survival Rate (%)")
plt.title("Survival Rate by Passenger Class")
plt.grid(True)
plt.show()
```

## Practical 08

### Aim 8: Organization of Data
```python
import pandas as pd
import numpy as np

df = pd.read_csv("/content/Social_Network_Ads.csv")
print('original shape: \n', df.shape)
df.head()

#after loading the data do data Inspection
print("\n Information \n")
print(df.info())
print("\n Missing values \n")
print(df.isna().sum())
print("\n Duplicate values \n")
print(df.duplicated().sum())

#Remove the duplicate records
df.drop_duplicates (inplace=True)
print('shape after removing duplicates: \n', df.shape)

#Handles missing values
for col in df.columns:
    if df[col].isna().sum()>0:
        if df[col].dtypes=='object':
            df[col].fillna(df[col].mode() [0], inplace=True)
        else:
            df[col].fillna(df[col].mean(), inplace=True)

print("\n Missing values \n")
print(df.isna().sum())

#Standardize Gender Values
df['Gender']=df['Gender'].map({'Male':1, 'Female':0})

#Rename Columns
df.rename(columns={'EstimatedSalary': 'Salary', 'Purchased': 'Purchase_Status'}, inplace=True)

#Ensure columns are renamed correctly and remove impossible values
rename_dict={'EstimatedSalary': 'Salary', 'Purchased': 'Purchase_Status'}
df.rename(columns={k: v for k, v in rename_dict.items() if k in df.columns}, inplace=True)

#Remove impossible values
df = df[
(df['Age'] >= 18) &
(df['Age'] <= 65) &
(df['Salary'] > 0)
]
print("Current columns:", df.columns.tolist())

# Bands using quantiles
df['Salary_Band'] = pd.qcut(df['Salary'], q=4, labels=["Low", "Medium", "High", "Very High"])

bins=[18, 30, 45, 65]
labels = ['Young', 'Adult', 'Senior']
df['Age_Group'] = pd.cut(df['Age'], bins=bins, labels=labels)

#Encode Gender
df['Gender_Code'] = df['Gender'].replace({'Male': 1, 'Female': 0})

#Normalize Salary
df['Salary_Normalized'] = (df['Salary'] - df['Salary'].min()) / (df['Salary'].max() - df['Salary'].min())

# Salary Rank
df['Salary_Rank'] = df['Salary'].rank(ascending=False, method="dense")

#Multi-level Sorting
df.sort_values(by=['Age_Group', 'Salary', 'Purchase_Status'], ascending=[True, False, False], inplace=True)

# Groups Statistics
summary = df.groupby(['Gender', 'Age_Group'], observed=False).agg(
    Total_Customers=("User ID", "count"),
    Average_Salary=("Salary", "mean"),
    Total_Salary=("Salary", "sum"),
    Max_Salary=("Salary", "max"),
    Min_Salary=("Salary", "min")
)
print("\n Group Summary")
print (summary)

# Pivot Table
pivot = pd.pivot_table(
    df,
    values='Salary',
    index='Age_Group',
    columns='Gender',
    aggfunc=['mean', 'max', 'min'],
    observed=False
)
print("\n Pivot Table")
print(pivot, "\n\n")

# MultiIndex Dataset
organized = df.set_index(["Gender", "Age_Group"])
print("\n Organized Dataset \n")
print(organized.head(), "\n\n")

#Detect Outliners
Q1 = df['Salary'].quantile(0.25)
Q3 = df['Salary'].quantile(0.75)
IQR = Q3 - Q1
Outliners = df[(df['Salary'] < Q1 - 1.5 * IQR) | (df['Salary'] > Q3 + 1.5 * IQR)]
print("in Outliners \n")
print (Outliners)

# Export files
try:
    organized.to_csv("organized_data.csv")
    summary.to_csv("group_summary.csv")
    pivot.to_csv("pivot_table.csv")
    Outliners.to_csv("outliners.csv")
    print("\n All files created successfully in /content/ \n")
except NameError as e:
    print(f"Error: {e}. Please ensure all processing cells have been executed.")
```

## Practical 09

### Aim 9: Generating Data
```python
import numpy as np
import pandas as pd
import faker

#Generate Personal Information
n = 1000
gender = np.random.choice(['Male', 'Female'], size=n, p=[0.55, 0.45])
age = np.random.randint(18, 61, size=n)

#Generate Salary
salary = (age * 3500 + np.random.randint(15000, 80000, size=n)).astype(int)
salary = np.clip(salary, 20000, 250000)

#Experience years
Exp = np.maximum(
    age - 22 + np.random.randint(-2, 4, size=n),
    0
)

#Generate Education
Edu = np.random.choice(
    ['Diploma', 'Bachelor', 'Masters', 'PhD'],
    size=n,
    p=[0.15, 0.50, 0.25, 0.10]
)

#Generate City
city = np.random.choice(
    ['Mumbai', 'Delhi', 'banglore', 'Hyderabad', 'Ahmedabad', 'Chennai', 'Kolkata', 'Pune', 'Jaipur', 'Surat'],
    size=n
)

#Generate Joining Dates
fake = faker.Faker()
join_date = [fake.date_between(start_date='-10y', end_date='today') for _ in range(n)]

#Purchase Probability
probability = (
    (salary / salary.max()) * 0.45
    + (Exp / Exp.max()) * 0.35
    + np.random.random(size=n) * 0.20
)
purchased = (probability >= 0.55).astype(int)

#Build Data Frame
user_Id = range(1, n + 1)
df = pd.DataFrame({
    'user_Id': user_Id,
    'gender': gender,
    'Age': age,
    'Salary': salary,
    'Education': Edu,
    'City': city,
    'Joining Date': join_date,
    'Purchased': purchased
})

# Introducing Missing Values
for column in ["Age", "Salary", "Education"]:
    missing_rows = np.random.choice(
        df.index,
        15,
        replace=False
    )
    df.loc[missing_rows, column] = np.nan

# Introduce Duplicate Records
duplicates = df.sample(20, random_state=42)
df = pd.concat(
    [df, duplicates],
    ignore_index=True
)

# Shuffle Dataset
df = df.sample(
    frac=1,
    random_state=42
).reset_index(
    drop=True
)

print("\n Dataset Shape")
print(df.shape)
print("\n Data Types")
print(df.dtypes)
print("in First Five Rows")
print(df.head())
print("\n Last Five Rows")
print(df.tail())
print("\n Missing values")
print(df.isnull().sum())
print("\n Statistical Summary")
print(df.describe(include="all"))

#Save Dataset
df.to_csv('Generated_Employee_Dataset.csv',index=False)
print("\n Dataset Generated successfully")
print("File Name: Generated_Employee_Dataset.csv")
print("File Location: /contents/Generated_Employee_Dataset.csv")
```






---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ///////////////////////////                    /////////////////////////////////////////         Soft Computing //////////////////////////////////////

````md
## 1A. Simple Linear Neural Network Model

### Aim
To design and implement a simple linear neural network model using the equation `y = Wx + b`.

### Code
```python
x = float(input("Enter Value for X: "))
W = float(input("Enter Value for W: "))
b = float(input("Enter Value for B: "))

y = (W * x) + b
print(f"Output y = {y}")
````

---

## 1B. Binary and Bipolar Sigmoidal Functions

### Aim

To calculate the output of a neural network using binary sigmoid and bipolar sigmoid activation functions.

### Code

```python
import math

x1 = float(input("X1="))
x2 = float(input("X2="))
x3 = float(input("X3="))

w1 = float(input("W1="))
w2 = float(input("W2="))
w3 = float(input("W3="))

yin = (x1 * w1) + (x2 * w2) + (x3 * w3)
print("Net Input is: ", round(yin, 4))

binary = 1 / (1 + math.exp(-yin))
print("Binary Sigmoid Output: ", round(binary, 4))

bipolar = (2 / (1 + math.exp(-yin))) - 1
print("Bipolar Sigmoid Output: ", round(bipolar, 4))
```

---

## 2A. AND/NOT Function Using McCulloch-Pitts Neural Network

### Aim

To implement AND/NOT logic functions using a McCulloch-Pitts neural network and a threshold activation function.

### Code

```python
import numpy as np

num_ip = int(input("Enter the number of inputs: "))

w1 = 1
w2 = 1

print("For the", num_ip, "inputs calculate the net input using yin = x1w1 + x2w2")

x1 = []
x2 = []

for j in range(0, num_ip):
    ele1 = int(input("x1 = "))
    ele2 = int(input("x2 = "))
    x1.append(ele1)
    x2.append(ele2)

print("x1 =", x1)
print("x2 =", x2)

n = np.array(x1) * w1
m = np.array(x2) * w2

Yin = []
for i in range(0, num_ip):
    Yin.append(n[i] + m[i])
print("Yin ", Yin)

Yin = []
for i in range(0, num_ip):
    Yin.append(n[i] - m[i])
print("After assuming one weight as excitatory and the other as inhibitory Yin ", Yin)

Y = []
for i in range(0, num_ip):
    if Yin[i] >= 1:
        ele = 1
    else:
        ele = 0
    Y.append(ele)

print("Y =", Y)
```

---

## 2B. XOR Function Using a Neural Network

### Aim

To implement the XOR function using a neural network containing input, hidden, and output nodes, and train it using error back-propagation.

### Code

```python
import math
import numpy
import random

INPUT_NODES = 2
OUTPUT_NODES = 1
HIDDEN_NODES = 2

MAX_ITERATIONS = 130000
LEARNING_RATE = .2

print("Neural Network Program")


class network:
    def __init__(self, input_nodes, hidden_nodes, output_nodes, learning_rate):
        self.input_nodes = input_nodes
        self.hidden_nodes = hidden_nodes
        self.output_nodes = output_nodes
        self.total_nodes = input_nodes + hidden_nodes + output_nodes
        self.learning_rate = learning_rate

        self.values = numpy.zeros(self.total_nodes)
        self.expectedValues = numpy.zeros(self.total_nodes)
        self.thresholds = numpy.zeros(self.total_nodes)
        self.weights = numpy.zeros((self.total_nodes, self.total_nodes))

        random.seed(10000)

        for i in range(self.input_nodes, self.total_nodes):
            self.thresholds[i] = random.random() / random.random()
            for j in range(i + 1, self.total_nodes):
                self.weights[i][j] = random.random() * 2

    def process(self):
        for i in range(self.input_nodes,
                       self.input_nodes + self.hidden_nodes):
            W_i = 0.0
            for j in range(self.input_nodes):
                W_i += self.weights[j][i] * self.values[j]
            W_i -= self.thresholds[i]
            self.values[i] = 1 / (1 + math.exp(-W_i))

        for i in range(self.input_nodes + self.hidden_nodes,
                       self.total_nodes):
            W_i = 0.0
            for j in range(self.input_nodes,
                           self.input_nodes + self.hidden_nodes):
                W_i += self.weights[j][i] * self.values[j]
            W_i -= self.thresholds[i]
            self.values[i] = 1 / (1 + math.exp(-W_i))

    def processErrors(self):
        sumOfSquaredErrors = 0.0

        for i in range(self.input_nodes + self.hidden_nodes,
                       self.total_nodes):
            error = self.expectedValues[i] - self.values[i]
            sumOfSquaredErrors += math.pow(error, 2)

            outputErrorGradient = (
                self.values[i] * (1 - self.values[i]) * error
            )

            for j in range(self.input_nodes,
                           self.input_nodes + self.hidden_nodes):
                delta = (
                    self.learning_rate
                    * self.values[j]
                    * outputErrorGradient
                )
                self.weights[j][i] += delta

                hiddenErrorGradient = (
                    self.values[j]
                    * (1 - self.values[j])
                    * outputErrorGradient
                    * self.weights[j][i]
                )

                for k in range(self.input_nodes):
                    delta = (
                        self.learning_rate
                        * self.values[k]
                        * hiddenErrorGradient
                    )
                    self.weights[k][j] += delta

                delta = (
                    self.learning_rate
                    * -1
                    * hiddenErrorGradient
                )
                self.thresholds[j] += delta

            delta = (
                self.learning_rate
                * -1
                * outputErrorGradient
            )
            self.thresholds[i] += delta

        return sumOfSquaredErrors


class sampleMaker:
    def __init__(self, network):
        self.counter = 0
        self.network = network

    def setXor(self, x):
        if x == 0:
            self.network.values[0] = 1
            self.network.values[1] = 1
            self.network.expectedValues[4] = 0
        elif x == 1:
            self.network.values[0] = 0
            self.network.values[1] = 1
            self.network.expectedValues[4] = 1
        elif x == 2:
            self.network.values[0] = 1
            self.network.values[1] = 0
            self.network.expectedValues[4] = 1
        else:
            self.network.values[0] = 0
            self.network.values[1] = 0
            self.network.expectedValues[4] = 0

    def setNextTrainingData(self):
        self.setXor(self.counter % 4)
        self.counter += 1


net = network(INPUT_NODES, HIDDEN_NODES, OUTPUT_NODES, LEARNING_RATE)
samples = sampleMaker(net)

for i in range(MAX_ITERATIONS):
    samples.setNextTrainingData()
    net.process()
    error = net.processErrors()

    if i > (MAX_ITERATIONS - 5):
        output = (
            net.values[0],
            net.values[1],
            net.values[4],
            net.expectedValues[4],
            error
        )
        print(output)

print(net.weights)
print(net.thresholds)
```

---

## 3A. Hebb's Rule

### Aim

To implement Hebb's learning rule for updating the weights and bias of a neural network.

### Code

```python
import numpy as np

x1 = np.array([1, 1, 1, -1, 1, -1, 1, 1, 1])
x2 = np.array([1, 1, 1, -1, 1, -1, 1, 1, 1])

b = 0
y = np.array([1, -1])

wtold = np.zeros((9,))
wtnew = np.zeros((9,))
wtnew = wtnew.astype(int)
wtold = wtold.astype(int)
bais = 0

print("First input with target=1")

for i in range(0, 9):
    wtold[i] = wtold[i] + x1[i] * y[0]

wtnew = wtold
b = b + y[0]

print("Second input with target=-1")

for i in range(0, 9):
    wtnew[i] = wtold[i] + x2[i] * y[1]

b = b + y[1]

print("new wt=", wtnew)
print("Bias value", b)
```

---

## 3B. Delta Rule

### Aim

To implement the Delta learning rule for adjusting neural network weights until the desired output is obtained.

### Code

```python
import numpy as np

np.set_printoptions(precision=2)

x = np.zeros((3,))
weights = np.zeros((3,))
desired = np.zeros((3,))
actual = np.zeros((3,))

for i in range(0, 3):
    x[i] = float(input("Initial Inputs:-"))

for i in range(0, 3):
    weights[i] = float(input("Initial weights:-"))

for i in range(0, 3):
    desired[i] = float(input("Desired output:-"))

a = float(input("Enter learning rate:-"))

actual = x * weights

print("Actual", actual)
print("Desired", desired)

while True:
    if np.array_equal(desired, actual):
        break
    else:
        for i in range(0, 3):
            weights[i] = weights[i] + a * (desired[i] - actual[i])

        actual = x * weights

        print("Weights", weights)
        print("Actual", actual)
        print("desired", desired)
        print("*" * 30)

print("Final Output")
print("Corrected weights", weights)
print("Actual", actual)
print("Desired", desired)
```

---

## 4A. Back Propagation Algorithm

### Aim

To implement the Back Propagation algorithm for calculating errors and updating the weights and biases of a multilayer neural network.

### Code

```python
import numpy as np
import math

np.set_printoptions(precision=2)

v1 = np.array([0.6, 0.3])
v2 = np.array([-0.1, 0.4])
w = np.array([-0.2, 0.4, 0.1])

b1 = 0.3
b2 = 0.5
x1 = 0
x2 = 1
alpha = 0.25

print("Calculate net input to z1 layer.")

zin1 = round(b1 + x1 * v1[0] + x2 * v2[0], 4)

print("zin1=", round(zin1, 3))

print("Calculate net input to z2 layer.")

zin2 = round(b2 + x1 * v1[1] + x2 * v2[1], 4)

print("zin2=", round(zin2, 4))

print("Apply activation function to calculate output.")

z1 = 1 / (1 + math.exp(-zin1))
z1 = round(z1, 4)

z2 = 1 / (1 + math.exp(-zin2))
z2 = round(z2, 4)

print("z1=", z1)
print("z2=", z2)

print("Calculate net input to output layer.")

yin = w[0] + z1 * w[1] + z2 * w[2]

print("yin=", yin)

print("Calculate net output.")

y = 1 / (1 + math.exp(-yin))

print("y=", y)

fyin = y * (1 - y)

dk = (1 - y) * fyin

print("dk=", dk)

dw1 = alpha * dk * z1
dw2 = alpha * dk * z2
dw0 = alpha * dk

print("Compute error portion in delta.")

din1 = dk * w[1]
din2 = dk * w[2]

print("din1=", din1)
print("din2=", din2)

print("Error in delta.")

fzin1 = z1 * (1 - z1)

print("fzin1=", fzin1)

d1 = din1 * fzin1

fzin2 = z2 * (1 - z2)

print("fzin2=", fzin2)

d2 = din2 * fzin2

print("d1=", d1)
print("d2=", d2)

print("Changes in weights between input and hidden layer.")

dv11 = alpha * d1 * x1

print("dv11=", dv11)

dv21 = alpha * d1 * x2

print("dv21=", dv21)

dv01 = alpha * d1

print("dv01=", dv01)

dv12 = alpha * d2 * x1

print("dv12=", dv12)

dv22 = alpha * d2 * x2

print("dv22=", dv22)

dv02 = alpha * d2

print("dv02=", dv02)

print("Final weights of network.")

v1 = v1.astype(float)
v2 = v2.astype(float)
w = w.astype(float)

v1[0] = v1[0] + dv11
v1[1] = v1[1] + dv12

print("v1=", v1)

v2[0] = v2[0] + dv21
v2[1] = v2[1] + dv22

print("v2=", v2)

w[1] = w[1] + dw1
w[2] = w[2] + dw2

b1 = b1 + dv01
b2 = b2 + dv02

w[0] = w[0] + dw0

print("w=", w)
print("bias b1=", b1, "b2=", b2)
```

---

## 4B. Error Back Propagation Algorithm

### Aim

To implement the error back-propagation algorithm using the hyperbolic tangent activation function and update the network weights and biases.

### Code

```python
import math

a0 = 1
t = 1

w10 = float(input("Enter First Weight:"))
b10 = float(input("Enter First Bias Weight:"))
w20 = float(input("Enter Second Weight:"))
b20 = float(input("Enter Second Bias Weight:"))

c = float(input("Enter Learning Coefficient:"))

n1 = float(w10 * c + b10)

a1 = math.tanh(float(n1))

n2 = float(w20 * a1 + b20)

a2 = math.tanh(float(n2))

e = t - a2

s2 = -2 * (1 - a2 * a2) * e

s1 = (1 - a1 * a1) * w20 * s2

w21 = w20 - (c * s2 * a1)

w11 = w10 - (c * s1 * a0)

b21 = b20 - (c * s2)

b11 = b10 - (c * s1)

print("The updated weight of first n/w w11=", w11)
print("The updated weight of second n/w w21=", w21)
print("The updated base of first n/w b10=", b10)
print("The updated base of second n/w b20=", b20)
```

---

## 5A. Hopfield Network

### Aim

To implement a Hopfield neural network with four fully interconnected neurons and verify whether the network correctly recalls stored patterns.

### Code

```python
class Neuron:

    def __init__(self, j):
        self.activation = 0
        self.weightv = [0] * 4

        for i in range(4):
            self.weightv[i] = j[i]

    def act(self, m, x):
        a = 0

        for i in range(m):
            a += x[i] * self.weightv[i]

        return a


class Network:

    def __init__(self, a, b, c, d):
        self.nrn = [None] * 4
        self.output = [0] * 4

        self.nrn[0] = Neuron(a)
        self.nrn[1] = Neuron(b)
        self.nrn[2] = Neuron(c)
        self.nrn[3] = Neuron(d)

    def threshold(self, k):
        if k >= 0:
            return 1
        else:
            return 0

    def activation(self, patrn):
        for i in range(4):

            for j in range(4):
                print(
                    "\n nrn[{}].weightv[{}] is {}".format(
                        i, j, self.nrn[i].weightv[j]
                    )
                )

            self.nrn[i].activation = self.nrn[i].act(4, patrn)

            print("\n Activation is:", self.nrn[i].activation)

            self.output[i] = self.threshold(
                self.nrn[i].activation
            )

            print("\n Output Value is: ", self.output[i])


def main():

    patrn1 = [1, 0, 1, 0]
    patrn2 = [0, 1, 0, 1]

    wt1 = [0, -3, 3, -3]
    wt2 = [-3, 0, -3, 3]
    wt3 = [3, -3, 0, -3]
    wt4 = [-3, 3, -3, 0]

    print(
        "\n THIS PROGRAM IS FOR A HOPFIELD NETWORK WITH A SINGLE LAYER OF:"
    )

    print(
        "\n 4 FULLY INTERCONNECTED NEURONS. THE NETWORK SHOULD RECALL "
        "THE PATTERNS 1010 AND 0101 CORRECTLY.\n"
    )

    h1 = Network(wt1, wt2, wt3, wt4)

    print("\n--- Testing Pattern 1: [1, 0, 1, 0] ---")

    h1.activation(patrn1)

    for i in range(4):

        if h1.output[i] == patrn1[i]:

            print(
                "\n PatternIn= ",
                patrn1[i],
                "Output =",
                h1.output[i],
                "Component matches",
            )

        else:

            print(
                "\n PatternIn= ",
                patrn1[i],
                "Output =",
                h1.output[i],
                "Discrepancy Occurred",
            )

    print("\n\n--- Testing Pattern 2: [0, 1, 0, 1] ---")

    h1.activation(patrn2)

    for i in range(4):

        if h1.output[i] == patrn2[i]:

            print(
                "\n PatternIn= ",
                patrn2[i],
                "Output =",
                h1.output[i],
                "Component matches",
            )

        else:

            print(
                "\n PatternIn= ",
                patrn2[i],
                "Output =",
                h1.output[i],
                "Discrepancy Occurred",
            )


if __name__ == "__main__":
    main()
```

---

## 5B. Radial Basis Function Network

### Aim

To implement a Radial Basis Function (RBF) neural network for function approximation and visualize the learned model.

### Code

```python
from scipy import *
from scipy.linalg import norm, pinv
import numpy as np
from matplotlib import pyplot as plt


class RBF:

    def __init__(self, indim, numCenters, outdim):

        self.indim = indim
        self.outdim = outdim
        self.numCenters = numCenters

        self.centers = [
            np.random.uniform(-1, 1, indim)
            for i in range(numCenters)
        ]

        self.beta = 8

        self.W = np.random.random(
            (self.numCenters, self.outdim)
        )

    def _basisfunc(self, c, d):

        assert len(d) == self.indim

        return np.exp(
            -self.beta * norm(c - d) ** 2
        )

    def _calcAct(self, X):

        G = np.zeros(
            (X.shape[0], self.numCenters),
            float
        )

        for ci, c in enumerate(self.centers):

            for xi, x in enumerate(X):

                G[xi, ci] = self._basisfunc(c, x)

        return G

    def train(self, X, Y):

        """X: matrix of dimensions n x indim
        Y: column vector of dimension n x 1"""

        rnd_idx = np.random.permutation(
            X.shape[0]
        )[:self.numCenters]

        self.centers = [
            X[i, :]
            for i in rnd_idx
        ]

        print("center", self.centers)

        G = self._calcAct(X)

        print(G)

        self.W = np.dot(
            pinv(G),
            Y
        )

    def test(self, X):

        """X: matrix of dimensions n x indim"""

        G = self._calcAct(X)

        Y = np.dot(
            G,
            self.W
        )

        return Y


if __name__ == '__main__':

    n = 100

    x = np.mgrid[
        -1:1:complex(0, n)
    ].reshape(n, 1)

    y = np.sin(
        3 * (x + 0.5) ** 3 - 1
    )

    rbf = RBF(
        1,
        10,
        1
    )

    rbf.train(
        x,
        y
    )

    z = rbf.test(x)

    plt.figure(
        figsize=(12, 8)
    )

    plt.plot(
        x,
        y,
        'k-'
    )

    plt.plot(
        x,
        z,
        'r-',
        linewidth=2
    )

    plt.plot(
        rbf.centers,
        np.zeros(rbf.numCenters),
        'gs'
    )

    for c in rbf.centers:

        cx = np.arange(
            c - 0.7,
            c + 0.7,
            0.01
        )

        cy = [
            rbf._basisfunc(
                np.array([cx_]),
                np.array([c])
            )
            for cx_ in cx
        ]

        plt.plot(
            cx,
            cy,
            '-',
            color='gray',
            linewidth=0.2
        )

    plt.xlim(
        -1.2,
        1.2
    )

    plt.show()
```

```
```




