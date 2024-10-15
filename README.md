import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Function to load the CSV data with proper encoding
def load_data(file_path):
    try:
        df = pd.read_csv(file_path, encoding='ISO-8859-1')  # Specify encoding to handle non-UTF-8 files
        df.columns = df.columns.str.strip()  # Clean up any whitespace in column names
        return df
    except Exception as e:
        print(f"Error loading file: {e}")
        return None

# Function to visualize numeric columns
def visualize_numeric(df, numeric_cols):
    print("\nVisualizing numeric columns...")
    for col in numeric_cols:
        plt.figure(figsize=(12, 6))
        sns.histplot(df[col], kde=True)  # Histogram with KDE
        plt.title(f'Distribution of {col}')
        plt.xlabel(col)
        plt.ylabel('Frequency')
        plt.grid(True)
        plt.tight_layout()
        plt.show()

# Function to visualize categorical columns
def visualize_categorical(df, categorical_cols):
    print("\nVisualizing categorical columns...")
    for col in categorical_cols:
        plt.figure(figsize=(12, 6))
        sns.countplot(data=df, x=col)
        plt.title(f'Distribution of {col}')
        plt.xlabel(col)
        plt.ylabel('Count')
        plt.xticks(rotation=45)
        plt.grid(True)
        plt.tight_layout()
        plt.show()

# Function to visualize data of all columns
def visualize_all_columns(df):
    print("\nVisualizing all columns...")

   # Identify column types
  numeric_cols = df.select_dtypes(include=['number']).columns.tolist()
    categorical_cols = df.select_dtypes(include=['object']).columns.tolist()
    # Visualize numeric columns if any
    if numeric_cols:
        visualize_numeric(df, numeric_cols)
    else:
        print("\nNo numeric columns found.")
    # Visualize categorical columns if any
    if categorical_cols:
        visualize_categorical(df, categorical_cols)
    else:
        print("\nNo categorical columns found.")

# Function to compare all numeric columns using a pie chart
def compare_all_columns_pie_chart(df):
    print("\nComparing all numeric columns using a pie chart...")
    # Only consider numeric columns
    numeric_cols = df.select_dtypes(include=['number']).columns.tolist()
    if numeric_cols:
        # Sum the values for each numeric column
        sums = df[numeric_cols].sum()
        # Plot the pie chart
        plt.figure(figsize=(8, 8))
        plt.pie(sums, labels=numeric_cols, autopct='%1.1f%%', startangle=90, colors=sns.color_palette("Set3"))
        plt.title("Comparison of All Numeric Columns (Sum)")
        plt.tight_layout()
        plt.show()
    else:
        print("\nNo numeric columns to compare in the pie chart.")

# Main function to analyze and visualize any CSV file
def analyze_and_visualize(file_path):
    df = load_data(file_path)
    if df is None:
        print("Failed to load data. Exiting.")
        return
    # Display data overview
    print("\nData Overview:")
    print(df.info())
    print(df.head())  # Display first few row
    # Visualize all columns
    visualize_all_columns(df)
    # Compare all numeric columns using a pie chart
    compare_all_columns_pie_chart(df)

# Entry point to the program
def main():
    file_path = input("Enter the path to your CSV file: ")
    analyze_and_visualize(file_path)

if __name__ == "__main__":
    main()
