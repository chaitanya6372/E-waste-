import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import io

def load_data_from_string(csv_data):
    """Load data from a CSV string"""
    return pd.read_csv(io.StringIO(csv_data))

def analyze_data(df):
    """Analyze e-waste data and return category counts"""
    category_counts = df['Category'].value_counts()
    return category_counts

def filter_data(df, category=None, min_count=0):
    """Filter data by category and minimum count"""
    filtered = df.copy()
    if category:
        filtered = filtered[filtered['Category'] == category]
    if min_count > 0:
        filtered = filtered[filtered['Count'] >= min_count]
    return filtered

def generate_report(df, output_file='e_waste_report.txt'):
    """Generate comprehensive text report"""
    category_counts = df['Category'].value_counts()

    with open(output_file, 'w') as f:
        f.write("E-Waste Analysis Report\n")
        f.write("="*40 + "\n")
        f.write(f"Total Categories: {len(category_counts)}\n")
        f.write(f"Total Items: {df['Count'].sum()}\n\n")

        f.write("Category Distribution:\n")
        # Use items() instead of iteritems() for newer pandas versions
        for category, count in category_counts.items():
            f.write(f"- {category}: {count} items\n")

        f.write("\nStatistics:\n")
        stats = df.groupby('Category')['Count'].describe()
        f.write(str(stats))

    print(f"Report saved as '{output_file}'")
    return stats

def create_visualizations(df):
    """Create all visualizations in a single figure"""
    plt.figure(figsize=(18, 12))

    # Bar plot
    plt.subplot(2, 2, 1)
    sns.barplot(x='Category', y='Count', data=df, palette='viridis')
    plt.title('E-Waste by Category')
    plt.xticks(rotation=45)

    # Pie chart
    plt.subplot(2, 2, 2)
    df.groupby('Category')['Count'].sum().plot.pie(autopct='%1.1f%%')
    plt.title('Category Distribution')

    # Box plot
    plt.subplot(2, 2, 3)
    sns.boxplot(x='Category', y='Count', data=df)
    plt.title('Distribution by Category')
    plt.xticks(rotation=45)

    # Scatter plot
    plt.subplot(2, 2, 4)
    sns.scatterplot(x='Category', y='Count', data=df, hue='Category', size='Count', sizes=(20, 200))
    plt.title('Quantity Variation')
    plt.xticks(rotation=45)

    plt.tight_layout()
    plt.savefig('e_waste_visualizations.png')
    plt.show()

def interactive_menu():
    """Interactive menu for analysis options"""
    print("\nE-Waste Analysis Tool")
    print("="*25)
    print("1. Show all data")
    print("2. Filter by category")
    print("3. Filter by minimum count")
    print("4. Generate report")
    print("5. Visualize data")
    print("6. Exit")

    while True:
        try:
            choice = int(input("Enter your choice (1-6): "))
            if 1 <= choice <= 6:
                return choice
            else:
                print("Invalid input. Please enter a number between 1 and 6.")
        except ValueError:
            print("Invalid input. Please enter a number.")


def main():
    try:
        # Your CSV data as a string
        csv_data = """Category,Count
Electronics,100
Batteries,50
Cables,75
Appliances,30"""

        # Load data from string
        df = load_data_from_string(csv_data)
        print("Data loaded successfully from string.")
        display(df) # Display the loaded dataframe


        while True:
            choice = interactive_menu()

            if choice == 1:
                print("\nAll Data:")
                print(df)

            elif choice == 2:
                category = input("Enter category name: ")
                filtered = filter_data(df, category=category)
                print(f"\nFiltered Data ({category}):")
                print(filtered)

            elif choice == 3:
                min_count = int(input("Enter minimum count: "))
                filtered = filter_data(df, min_count=min_count)
                print(f"\nItems with count ≥ {min_count}:")
                print(filtered)

            elif choice == 4:
                stats = generate_report(df)
                print("\nSummary Statistics:")
                print(stats)

            elif choice == 5:
                create_visualizations(df)

            elif choice == 6:
                print("Exiting program...")
                break

    except Exception as e:
        print(f"Error: {e}")

if __name__ == "__main__":
    main()
