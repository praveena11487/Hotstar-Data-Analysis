# Hotstar-Data-Analysis
A comprehensive data analysis of the Disney+ Hotstar content library using Python. Includes data cleaning, trend identification, and visualizations of content distribution to uncover audience viewing patterns.
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_style("whitegrid")
sns.set_context("notebook")

plt.rcParams['figure.titlesize'] = 24
plt.rcParams['axes.titlesize'] = 15
plt.rcParams['axes.titleweight'] = 'bold'
plt.rcParams['axes.labelsize'] = 12
plt.rcParams['xtick.labelsize'] = 10
plt.rcParams['ytick.labelsize'] = 10
plt.rcParams['legend.fontsize'] = 10

df = pd.read_csv("Hotstar_Dataset.csv")

df['director'] = df['director'].fillna('Unknown')
df['country'] = df['country'].fillna('Unknown')
df['cast'] = df['cast'].fillna('Unknown')
df['rating'] = df['rating'].fillna('Not Rated')

df.drop_duplicates(inplace=True)

avg_year = np.mean(df['release_year'])

total_records = len(df)
total_movies = len(df[df['type'] == 'Movie'])
total_tvshows = len(df[df['type'] == 'TV Show'])

print("Total Records :", total_records)
print("Movies :", total_movies)
print("TV Shows :", total_tvshows)
print("Average Release Year :", round(avg_year, 2))

fig, axes = plt.subplots(
    3,
    3,
    figsize=(24, 18),
    constrained_layout=True,
    facecolor='white'
)

fig.suptitle(
    "JIO HOTSTAR DATA ANALYSIS DASHBOARD",
    fontsize=24,
    fontweight='bold'
)

for row in axes:
    for ax in row:
        
        ax.set_facecolor('#F8F9FA')
        ax.grid(alpha=0.3)

# --- PLOTS ---
sns.countplot(
    data=df,
    x='type',
    ax=axes[0,0]
)
axes[0,0].set_title("Movies vs TV Shows")
axes[0,0].set_xlabel("Content Type")
axes[0,0].set_ylabel("Count")

#----------------------------
top_country = df['country'].value_counts().head(10)
sns.barplot(
    x = top_country.values,
    y=top_country.index,
    ax=axes[0,1])
axes[0,1].set_title("Top 10 Countries")
axes[0,1].set_xlabel("Count")
axes[0,1].set_ylabel("County")
#----------------------------
sns.countplot(
    data=df,
    y='rating',
order=df['rating'].value_counts().index[:10],
    ax=axes[0,2]
)
axes[0,2].set_title("Top Ratings")
axes[0,2].set_xlabel("Count")
axes[0,2].set_ylabel("Ratings")
#----------------------------
year_data = df['release_year'].value_counts().sort_index()
axes[1, 0].plot(
    year_data.index,
    year_data.values,
    marker='o',
    linewidth=2
)
axes[1,0].set_title("Content Growth by Year")
axes[1,0].set_xlabel("Release Year")
axes[1,0].set_ylabel("Count")
#---------------------------
genres = df['listed_in'].str.split(',').explode().str.strip()
top_genres = genres.value_counts().head(10)
sns.barplot(
    x=top_genres.values,
    y=top_genres.index,
    ax=axes[1,1]
)
axes[1,1].set_title("Top Genres")
axes[1,1].set_xlabel("Count")
axes[1,1].set_ylabel("Genre")
#---------------------------
type_counts = df['type'].value_counts()
axes[1,2].pie(
    type_counts.values,
    labels=type_counts.index,
    autopct='%1.1f%%',
    startangle=90,
    
)
#---------------------------
top_years = df['release_year'].value_counts().head(10)
sns.barplot(
    x=top_years.index.astype(str),
    y=top_years.values,
    ax=axes[2, 0]
)
axes[1,2].set_title("Content Percentage")
axes[2, 0].set_title("Top 10 Release Years")
axes[2,0].set_xlabel("Year")
axes[2,0].set_ylabel("Count")
axes[2,0].tick_params(axis='x', rotation=45)
#----------------------------
country_data = df['country'].value_counts().head(5)
axes[2, 1].pie(
    country_data.values,
    labels=country_data.index,
    autopct='%1.1f%%',
    startangle=90,
    pctdistance=0.65,
    labeldistance=1.35,
    textprops={'fontsize':8},
    wedgeprops={'edgecolor':'black',
                'linewidth':1}
)
for ax in [axes[1,2], axes[2,1]]:
    ax.set_frame_on(True)
    for spine in ax.spines.values():
        spine.set_visible(True)
        spine.set_color('black')
        spine.set_linewidth(0.5)
axes[2,1].set_title("Top 5 Countries Share")
axes[2,1].patch.set_edgecolor('black')
axes[2,1].patch.set_linewidth(2)
for spine in axes[2,1].spines.values():
    spine.set_visible(True)
#---------------------------
    

# Project Summary
axes[2,2].axis('off')

summary_text = (f"Total Records    : {total_records}\n"
                f"Total Movies     : {total_movies}\n"
                f"Total TV Shows   : {total_tvshows}\n"
                f"Avg Release Year : {round(avg_year,2)}\n"
                f"Common Rating    : {df['rating'].mode()[0]}")

axes[2, 2].text(
    0.5,
    0.5,
    summary_text,
    ha='center',
    va='center',
    fontsize=12,
    fontweight='bold',
    bbox=dict(boxstyle='round,pad=1.5',
    facecolor='#D6EAF8',
    edgecolor='#3498DB',
    linewidth=2)

)
axes[2, 2].set_title(
    "Project Summary",
    pad=20
)
axes[2, 2].axis('off')


plt.show()

![My Visualization](JIO HOTSTAR DATA ANALYSIS DASHBOARD.jpg)


