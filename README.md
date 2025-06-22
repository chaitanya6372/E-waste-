import pandas as pd
import os
import matplotlib.pyplot as plt

# Define the path to the dataset
data_path = r'C:\Users\Sireesha\Downloads\E waste data\modified-dataset'

# Function to load data
def load_data(file_name):
    file_path = os.path.join(data_path, file_name)
    if os.path.exists(file_path):
        return pd.read_csv(file_path)
    else:
        raise FileNotFoundError(f"The file {file_name} does not exist in the specified path.")

# Function to analyze e-waste data
def analyze_data(df):
    # Example analysis: Count the number of entries by category
    category_counts = df['Category'].value_counts()
    return category_counts

# Function to generate a report
def generate_report(category_counts):
    report_path = os.path.join(data_path, 'e_waste_report.txt')
    with open(report_path, 'w') as f:
        f.write("E-Waste Analysis Report\n")
        f.write("========================\n")
        for category, count in category_counts.items():
            f.write(f"{category}: {count}\n")
    print(f"Report generated at: {report_path}")

# Function to visualize data
def visualize_data(category_counts):
    plt.figure(figsize=(10, 6))
    category_counts.plot(kind='bar', color='skyblue')
    plt.title('E-Waste Categories')
    plt.xlabel('Category')
    plt.ylabel('Count')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.savefig(os.path.join(data_path, 'e_waste_visualization.png'))
    plt.show()

# Main function
def main():
    try:
        # Load the dataset
        df = load_data('e_waste_data.csv')  # Replace with your actual CSV file name
        print("Data loaded successfully.")
        
        # Analyze the data
        category_counts = analyze_data(df)
        print("Data analysis completed.")
        
        # Generate a report
        generate_report(category_counts)
        
        # Visualize the data
        visualize_data(category_counts)
        
    except Exception as e:
        print(f"An error occurred: {e}")

if __name__ == "__main__":
    main()
import io

  g = sns.FacetGrid(df, col='Category', col_wrap=2)
  g.map(sns.barplot, 'Category', 'Count')
  g.set_titles(col_template="{col_name}")
  plt.show()
Cables,75
Appliances,30"""

df_from_string = pd.read_csv(io.StringIO(csv_data))
print("DataFrame created from string data:")
display(df_from_string)

  plt.scatter(df['Category'], df['Count'])
  plt.title('Scatter Plot of Product Count')
  plt.xlabel('Category')
  plt.ylabel('Count')
  plt.show()

  import seaborn as sns
  import matplotlib.pyplot as plt
  import pandas as pd
    data = {
        'Category': ['Appliances', 'Batteries', 'Cables', 'Electronics'],
        'Count': [30, 50, 75, 100]
    }
    df = pd.DataFrame(data)
    sns.barplot(x='Category', y='Count', data=df)
    plt.title('Product Count by Category')
    plt.show()
    
