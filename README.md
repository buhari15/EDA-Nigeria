# Exploratoty Data Analysis, And Visualization On Nigerian Inflation, GDP, GDP per capita and Population
## Data Source: World Bank
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Boxplot Normal Distribution Notebook: https://github.com/mGalarnyk/Python_Tutorials/blob/master/Statistics/boxplot/box_plot.ipynb"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Boxplot Interpretation"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Data taken from https://www.kaggle.com/uciml/breast-cancer-wisconsin-data/version/2"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "# Import all libraries for the rest of the blog post\n",
    "from scipy.integrate import quad\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "\n",
    "%matplotlib inline"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Load Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "df = pd.read_csv('https://raw.githubusercontent.com/mGalarnyk/Python_Tutorials/master/Kaggle/BreastCancerWisconsin/data/data.csv')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>diagnosis</th>\n",
       "      <th>radius_mean</th>\n",
       "      <th>texture_mean</th>\n",
       "      <th>perimeter_mean</th>\n",
       "      <th>area_mean</th>\n",
       "      <th>smoothness_mean</th>\n",
       "      <th>compactness_mean</th>\n",
       "      <th>concavity_mean</th>\n",
       "      <th>concave points_mean</th>\n",
       "      <th>...</th>\n",
       "      <th>texture_worst</th>\n",
       "      <th>perimeter_worst</th>\n",
       "      <th>area_worst</th>\n",
       "      <th>smoothness_worst</th>\n",
       "      <th>compactness_worst</th>\n",
       "      <th>concavity_worst</th>\n",
       "      <th>concave points_worst</th>\n",
       "      <th>symmetry_worst</th>\n",
       "      <th>fractal_dimension_worst</th>\n",
       "      <th>Unnamed: 32</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>842302</td>\n",
       "      <td>M</td>\n",
       "      <td>17.99</td>\n",
       "      <td>10.38</td>\n",
       "      <td>122.8</td>\n",
       "      <td>1001.0</td>\n",
       "      <td>0.1184</td>\n",
       "      <td>0.2776</td>\n",
       "      <td>0.3001</td>\n",
       "      <td>0.1471</td>\n",
       "      <td>...</td>\n",
       "      <td>17.33</td>\n",
       "      <td>184.6</td>\n",
       "      <td>2019.0</td>\n",
       "      <td>0.1622</td>\n",
       "      <td>0.6656</td>\n",
       "      <td>0.7119</td>\n",
       "      <td>0.2654</td>\n",
       "      <td>0.4601</td>\n",
       "      <td>0.1189</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>1 rows × 33 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "       id diagnosis  radius_mean  texture_mean  perimeter_mean  area_mean  \\\n",
       "0  842302         M        17.99         10.38           122.8     1001.0   \n",
       "\n",
       "   smoothness_mean  compactness_mean  concavity_mean  concave points_mean  \\\n",
       "0           0.1184            0.2776          0.3001               0.1471   \n",
       "\n",
       "      ...       texture_worst  perimeter_worst  area_worst  smoothness_worst  \\\n",
       "0     ...               17.33            184.6      2019.0            0.1622   \n",
       "\n",
       "   compactness_worst  concavity_worst  concave points_worst  symmetry_worst  \\\n",
       "0             0.6656           0.7119                0.2654          0.4601   \n",
       "\n",
       "   fractal_dimension_worst  Unnamed: 32  \n",
       "0                   0.1189          NaN  \n",
       "\n",
       "[1 rows x 33 columns]"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.head(1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Exploratory Data Analysis"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "B    357\n",
       "M    212\n",
       "Name: diagnosis, dtype: int64"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Looking at the Distribution of the Dataset in terms of Diagnosis\n",
    "df['diagnosis'].value_counts(dropna = False)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The section below is so that we can compare test performance with null accuracy"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The malignant percentage is: 37.2583479789%\n",
      "The benign percentage is: 62.7416520211%\n"
     ]
    }
   ],
   "source": [
    "length = len(df)\n",
    "\n",
    "# Number of malignant cases\n",
    "malignant = len(df[df['diagnosis']=='M'])\n",
    "\n",
    "#Rate of malignant tumors over all cases\n",
    "rate = (float(malignant)/(length))*100\n",
    "\n",
    "print('The malignant percentage is: {}%'.format(rate))\n",
    "print('The benign percentage is: {}%'.format(100 - rate))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "It is also possible to create a scatter matrix with the features. The red dots correspond to malignant diagnosis and blue to benign. Look how in some cases reds and blues dots occupies different regions of the plots. <b>This might not be useful with so many features</b>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Look at Boxplot"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {
    "collapsed": false
   },
   "outputs": [],
   "source": [
    "features = set(df.columns)\n",
    "features.remove('diagnosis')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Seaborn"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x10c674650>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.boxplot(x='diagnosis', y='area_mean', data=df)\n",
    "plt.savefig('seaborn_basic_area_mean_diagnosis.png')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Matplotlib"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x10c71f8d0>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "malignant = df[df['diagnosis']=='M']['area_mean']\n",
    "benign = df[df['diagnosis']=='B']['area_mean']\n",
    "\n",
    "fig = plt.figure()\n",
    "ax = fig.add_subplot(111)\n",
    "ax.boxplot([malignant,benign], labels=['M', 'B'])\n",
    "\n",
    "plt.savefig('matplotlib_basic_area_mean_diagnosis.png');"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Pandas"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\nTkSuQmCC\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x105762950>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "df.boxplot(column = 'area_mean', by = 'diagnosis');\n",
    "plt.title('')\n",
    "plt.savefig('pandas_basic_area_mean_diagnosis.png')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Nicer Seaborn"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x10c674d90>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize=(5,5))\n",
    "\n",
    "sns.boxplot(x='diagnosis', y='area_mean', data=df, palette=\"Set1\")\n",
    "\n",
    "# Changing default seaborn/matplotlib to be more readable\n",
    "plt.xlabel('diagnosis', fontsize = 24)\n",
    "plt.ylabel('area_mean', fontsize = 24)\n",
    "plt.xticks(fontsize = 20)\n",
    "plt.yticks(fontsize = 20)\n",
    "\n",
    "plt.savefig('area_mean_diagnosis.png')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Notched Boxplot"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "malignant = df[df['diagnosis']=='M']['area_mean']\n",
    "benign = df[df['diagnosis']=='B']['area_mean']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\nIFD/B1gvvImA6aX4AAAAAElFTkSuQmCC\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a105a34d0>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.figure()\n",
    "ax = fig.add_subplot(111)\n",
    "ax.boxplot([malignant,benign], notch = True, labels=['M', 'B']);"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Nicer Notched Boxplot "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a10017790>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "malignant = df[df['diagnosis']=='M']['area_mean']\n",
    "benign = df[df['diagnosis']=='B']['area_mean']\n",
    "\n",
    "fig = plt.figure(figsize = (8,5))\n",
    "ax = fig.add_subplot(111)\n",
    "boxplots = ax.boxplot([malignant,benign],\n",
    "           notch = True,\n",
    "           labels=['M', 'B'],\n",
    "           widths = .7,\n",
    "           patch_artist=True,\n",
    "           medianprops = dict(linestyle='-', linewidth=2, color='Yellow'),\n",
    "           boxprops = dict(linestyle='--', linewidth=2, color='Black', facecolor = 'blue', alpha = .4)\n",
    "          );\n",
    "\n",
    "boxplot1 = boxplots['boxes'][0]\n",
    "boxplot1.set_facecolor('red')\n",
    "\n",
    "plt.xlabel('diagnosis', fontsize = 20);\n",
    "plt.ylabel('area_mean', fontsize = 20);\n",
    "plt.xticks(fontsize = 16);\n",
    "plt.yticks(fontsize = 16);\n",
    "\n",
    "plt.savefig('nicer_notchedBoxplot_basic_area_mean_diagnosis.png')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Stackoverflow"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Get data out of boxplot: https://stackoverflow.com/questions/23349626/getting-data-of-a-box-plot-matplotlib?noredirect=1&lq=1"
   ]
  }
 ],
 "metadata": {
  "anaconda-cloud": {},
  "kernelspec": {
   "display_name": "Python [conda root]",
   "language": "python",
   "name": "conda-root-py"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 2
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython2",
   "version": "2.7.6"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 1
}

