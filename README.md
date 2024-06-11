# edx-project
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "<p style=\"text-align:center\">\n",
    "    <a href=\"https://skills.network/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork1047-2023-01-01\">\n",
    "    <img src=\"https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png\" width=\"200\" alt=\"Skills Network Logo\"  />\n",
    "    </a>\n",
    "</p>\n",
    "\n",
    "\n",
    "# Classification with Python\n",
    "\n",
    "\n",
    "Estimated time needed: **25** minutes\n",
    "    \n",
    "\n",
    "## Objectives\n",
    "\n",
    "After completing this lab you will be able to:\n",
    "\n",
    "* Confidently create classification models\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "In this notebook we try to practice all the classification algorithms that we learned in this course.\n",
    "\n",
    "We load a dataset using Pandas library, apply the following algorithms, and find the best one for this specific dataset by accuracy evaluation methods.\n",
    "\n",
    "Let's first load required libraries:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/utils/validation.py:37: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  LARGE_SPARSE_SUPPORTED = LooseVersion(scipy_version) >= '0.14.0'\n"
     ]
    }
   ],
   "source": [
    "import itertools\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "from matplotlib.ticker import NullFormatter\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib.ticker as ticker\n",
    "from sklearn import preprocessing\n",
    "%matplotlib inline"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### About dataset\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "This dataset is about the performance of basketball teams. The __cbb.csv__ data set includes performance data about five seasons of 354 basketball teams. It includes the following fields:\n",
    "\n",
    "| Field          | Description                                                                           |\n",
    "|----------------|---------------------------------------------------------------------------------------|\n",
    "|TEAM |\tThe Division I college basketball school|\n",
    "|CONF|\tThe Athletic Conference in which the school participates in (A10 = Atlantic 10, ACC = Atlantic Coast Conference, AE = America East, Amer = American, ASun = ASUN, B10 = Big Ten, B12 = Big 12, BE = Big East, BSky = Big Sky, BSth = Big South, BW = Big West, CAA = Colonial Athletic Association, CUSA = Conference USA, Horz = Horizon League, Ivy = Ivy League, MAAC = Metro Atlantic Athletic Conference, MAC = Mid-American Conference, MEAC = Mid-Eastern Athletic Conference, MVC = Missouri Valley Conference, MWC = Mountain West, NEC = Northeast Conference, OVC = Ohio Valley Conference, P12 = Pac-12, Pat = Patriot League, SB = Sun Belt, SC = Southern Conference, SEC = South Eastern Conference, Slnd = Southland Conference, Sum = Summit League, SWAC = Southwestern Athletic Conference, WAC = Western Athletic Conference, WCC = West Coast Conference)|\n",
    "|G|\tNumber of games played|\n",
    "|W|\tNumber of games won|\n",
    "|ADJOE|\tAdjusted Offensive Efficiency (An estimate of the offensive efficiency (points scored per 100 possessions) a team would have against the average Division I defense)|\n",
    "|ADJDE|\tAdjusted Defensive Efficiency (An estimate of the defensive efficiency (points allowed per 100 possessions) a team would have against the average Division I offense)|\n",
    "|BARTHAG|\tPower Rating (Chance of beating an average Division I team)|\n",
    "|EFG_O|\tEffective Field Goal Percentage Shot|\n",
    "|EFG_D|\tEffective Field Goal Percentage Allowed|\n",
    "|TOR|\tTurnover Percentage Allowed (Turnover Rate)|\n",
    "|TORD|\tTurnover Percentage Committed (Steal Rate)|\n",
    "|ORB|\tOffensive Rebound Percentage|\n",
    "|DRB|\tDefensive Rebound Percentage|\n",
    "|FTR|\tFree Throw Rate (How often the given team shoots Free Throws)|\n",
    "|FTRD|\tFree Throw Rate Allowed|\n",
    "|2P_O|\tTwo-Point Shooting Percentage|\n",
    "|2P_D|\tTwo-Point Shooting Percentage Allowed|\n",
    "|3P_O|\tThree-Point Shooting Percentage|\n",
    "|3P_D|\tThree-Point Shooting Percentage Allowed|\n",
    "|ADJ_T|\tAdjusted Tempo (An estimate of the tempo (possessions per 40 minutes) a team would have against the team that wants to play at an average Division I tempo)|\n",
    "|WAB|\tWins Above Bubble (The bubble refers to the cut off between making the NCAA March Madness Tournament and not making it)|\n",
    "|POSTSEASON|\tRound where the given team was eliminated or where their season ended (R68 = First Four, R64 = Round of 64, R32 = Round of 32, S16 = Sweet Sixteen, E8 = Elite Eight, F4 = Final Four, 2ND = Runner-up, Champion = Winner of the NCAA March Madness Tournament for that given year)|\n",
    "|SEED|\tSeed in the NCAA March Madness Tournament|\n",
    "|YEAR|\tSeason\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Load Data From CSV File  \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let's load the dataset [NB Need to provide link to csv file]\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
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
       "      <th>TEAM</th>\n",
       "      <th>CONF</th>\n",
       "      <th>G</th>\n",
       "      <th>W</th>\n",
       "      <th>ADJOE</th>\n",
       "      <th>ADJDE</th>\n",
       "      <th>BARTHAG</th>\n",
       "      <th>EFG_O</th>\n",
       "      <th>EFG_D</th>\n",
       "      <th>TOR</th>\n",
       "      <th>...</th>\n",
       "      <th>FTRD</th>\n",
       "      <th>2P_O</th>\n",
       "      <th>2P_D</th>\n",
       "      <th>3P_O</th>\n",
       "      <th>3P_D</th>\n",
       "      <th>ADJ_T</th>\n",
       "      <th>WAB</th>\n",
       "      <th>POSTSEASON</th>\n",
       "      <th>SEED</th>\n",
       "      <th>YEAR</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>North Carolina</td>\n",
       "      <td>ACC</td>\n",
       "      <td>40</td>\n",
       "      <td>33</td>\n",
       "      <td>123.3</td>\n",
       "      <td>94.9</td>\n",
       "      <td>0.9531</td>\n",
       "      <td>52.6</td>\n",
       "      <td>48.1</td>\n",
       "      <td>15.4</td>\n",
       "      <td>...</td>\n",
       "      <td>30.4</td>\n",
       "      <td>53.9</td>\n",
       "      <td>44.6</td>\n",
       "      <td>32.7</td>\n",
       "      <td>36.2</td>\n",
       "      <td>71.7</td>\n",
       "      <td>8.6</td>\n",
       "      <td>2ND</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Villanova</td>\n",
       "      <td>BE</td>\n",
       "      <td>40</td>\n",
       "      <td>35</td>\n",
       "      <td>123.1</td>\n",
       "      <td>90.9</td>\n",
       "      <td>0.9703</td>\n",
       "      <td>56.1</td>\n",
       "      <td>46.7</td>\n",
       "      <td>16.3</td>\n",
       "      <td>...</td>\n",
       "      <td>30.0</td>\n",
       "      <td>57.4</td>\n",
       "      <td>44.1</td>\n",
       "      <td>36.2</td>\n",
       "      <td>33.9</td>\n",
       "      <td>66.7</td>\n",
       "      <td>8.9</td>\n",
       "      <td>Champions</td>\n",
       "      <td>2.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Notre Dame</td>\n",
       "      <td>ACC</td>\n",
       "      <td>36</td>\n",
       "      <td>24</td>\n",
       "      <td>118.3</td>\n",
       "      <td>103.3</td>\n",
       "      <td>0.8269</td>\n",
       "      <td>54.0</td>\n",
       "      <td>49.5</td>\n",
       "      <td>15.3</td>\n",
       "      <td>...</td>\n",
       "      <td>26.0</td>\n",
       "      <td>52.9</td>\n",
       "      <td>46.5</td>\n",
       "      <td>37.4</td>\n",
       "      <td>36.9</td>\n",
       "      <td>65.5</td>\n",
       "      <td>2.3</td>\n",
       "      <td>E8</td>\n",
       "      <td>6.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Virginia</td>\n",
       "      <td>ACC</td>\n",
       "      <td>37</td>\n",
       "      <td>29</td>\n",
       "      <td>119.9</td>\n",
       "      <td>91.0</td>\n",
       "      <td>0.9600</td>\n",
       "      <td>54.8</td>\n",
       "      <td>48.4</td>\n",
       "      <td>15.1</td>\n",
       "      <td>...</td>\n",
       "      <td>33.4</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.3</td>\n",
       "      <td>40.3</td>\n",
       "      <td>34.7</td>\n",
       "      <td>61.9</td>\n",
       "      <td>8.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Kansas</td>\n",
       "      <td>B12</td>\n",
       "      <td>37</td>\n",
       "      <td>32</td>\n",
       "      <td>120.9</td>\n",
       "      <td>90.4</td>\n",
       "      <td>0.9662</td>\n",
       "      <td>55.7</td>\n",
       "      <td>45.1</td>\n",
       "      <td>17.8</td>\n",
       "      <td>...</td>\n",
       "      <td>37.3</td>\n",
       "      <td>52.7</td>\n",
       "      <td>43.4</td>\n",
       "      <td>41.3</td>\n",
       "      <td>32.5</td>\n",
       "      <td>70.1</td>\n",
       "      <td>11.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 24 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "             TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  \\\n",
       "0  North Carolina  ACC  40  33  123.3   94.9   0.9531   52.6   48.1  15.4   \n",
       "1       Villanova   BE  40  35  123.1   90.9   0.9703   56.1   46.7  16.3   \n",
       "2      Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3   \n",
       "3        Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1   \n",
       "4          Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8   \n",
       "\n",
       "   ...  FTRD  2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  \n",
       "0  ...  30.4  53.9  44.6  32.7  36.2   71.7   8.6         2ND   1.0  2016  \n",
       "1  ...  30.0  57.4  44.1  36.2  33.9   66.7   8.9   Champions   2.0  2016  \n",
       "2  ...  26.0  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016  \n",
       "3  ...  33.4  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016  \n",
       "4  ...  37.3  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016  \n",
       "\n",
       "[5 rows x 24 columns]"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df = pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ML0101EN-SkillsNetwork/labs/Module%206/cbb.csv')\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(1406, 24)"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Add Column\n",
    "Next we'll add a column that will contain \"true\" if the wins above bubble are over 7 and \"false\" if not. We'll call this column Win Index or \"windex\" for short. \n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "df['windex'] = np.where(df.WAB > 7, 'True', 'False')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "# Data visualization and pre-processing\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Next we'll filter the data set to the teams that made the Sweet Sixteen, the Elite Eight, and the Final Four in the post season. We'll also create a new dataframe that will hold the values with the new column.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
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
       "      <th>TEAM</th>\n",
       "      <th>CONF</th>\n",
       "      <th>G</th>\n",
       "      <th>W</th>\n",
       "      <th>ADJOE</th>\n",
       "      <th>ADJDE</th>\n",
       "      <th>BARTHAG</th>\n",
       "      <th>EFG_O</th>\n",
       "      <th>EFG_D</th>\n",
       "      <th>TOR</th>\n",
       "      <th>...</th>\n",
       "      <th>2P_O</th>\n",
       "      <th>2P_D</th>\n",
       "      <th>3P_O</th>\n",
       "      <th>3P_D</th>\n",
       "      <th>ADJ_T</th>\n",
       "      <th>WAB</th>\n",
       "      <th>POSTSEASON</th>\n",
       "      <th>SEED</th>\n",
       "      <th>YEAR</th>\n",
       "      <th>windex</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Notre Dame</td>\n",
       "      <td>ACC</td>\n",
       "      <td>36</td>\n",
       "      <td>24</td>\n",
       "      <td>118.3</td>\n",
       "      <td>103.3</td>\n",
       "      <td>0.8269</td>\n",
       "      <td>54.0</td>\n",
       "      <td>49.5</td>\n",
       "      <td>15.3</td>\n",
       "      <td>...</td>\n",
       "      <td>52.9</td>\n",
       "      <td>46.5</td>\n",
       "      <td>37.4</td>\n",
       "      <td>36.9</td>\n",
       "      <td>65.5</td>\n",
       "      <td>2.3</td>\n",
       "      <td>E8</td>\n",
       "      <td>6.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Virginia</td>\n",
       "      <td>ACC</td>\n",
       "      <td>37</td>\n",
       "      <td>29</td>\n",
       "      <td>119.9</td>\n",
       "      <td>91.0</td>\n",
       "      <td>0.9600</td>\n",
       "      <td>54.8</td>\n",
       "      <td>48.4</td>\n",
       "      <td>15.1</td>\n",
       "      <td>...</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.3</td>\n",
       "      <td>40.3</td>\n",
       "      <td>34.7</td>\n",
       "      <td>61.9</td>\n",
       "      <td>8.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>True</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Kansas</td>\n",
       "      <td>B12</td>\n",
       "      <td>37</td>\n",
       "      <td>32</td>\n",
       "      <td>120.9</td>\n",
       "      <td>90.4</td>\n",
       "      <td>0.9662</td>\n",
       "      <td>55.7</td>\n",
       "      <td>45.1</td>\n",
       "      <td>17.8</td>\n",
       "      <td>...</td>\n",
       "      <td>52.7</td>\n",
       "      <td>43.4</td>\n",
       "      <td>41.3</td>\n",
       "      <td>32.5</td>\n",
       "      <td>70.1</td>\n",
       "      <td>11.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>True</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Oregon</td>\n",
       "      <td>P12</td>\n",
       "      <td>37</td>\n",
       "      <td>30</td>\n",
       "      <td>118.4</td>\n",
       "      <td>96.2</td>\n",
       "      <td>0.9163</td>\n",
       "      <td>52.3</td>\n",
       "      <td>48.9</td>\n",
       "      <td>16.1</td>\n",
       "      <td>...</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.1</td>\n",
       "      <td>34.4</td>\n",
       "      <td>36.2</td>\n",
       "      <td>69.0</td>\n",
       "      <td>6.7</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Syracuse</td>\n",
       "      <td>ACC</td>\n",
       "      <td>37</td>\n",
       "      <td>23</td>\n",
       "      <td>111.9</td>\n",
       "      <td>93.6</td>\n",
       "      <td>0.8857</td>\n",
       "      <td>50.0</td>\n",
       "      <td>47.3</td>\n",
       "      <td>18.1</td>\n",
       "      <td>...</td>\n",
       "      <td>47.2</td>\n",
       "      <td>48.1</td>\n",
       "      <td>36.0</td>\n",
       "      <td>30.7</td>\n",
       "      <td>65.5</td>\n",
       "      <td>-0.3</td>\n",
       "      <td>F4</td>\n",
       "      <td>10.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>False</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 25 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "         TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  ...  \\\n",
       "2  Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3  ...   \n",
       "3    Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1  ...   \n",
       "4      Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8  ...   \n",
       "5      Oregon  P12  37  30  118.4   96.2   0.9163   52.3   48.9  16.1  ...   \n",
       "6    Syracuse  ACC  37  23  111.9   93.6   0.8857   50.0   47.3  18.1  ...   \n",
       "\n",
       "   2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  windex  \n",
       "2  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016   False  \n",
       "3  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016    True  \n",
       "4  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016    True  \n",
       "5  52.6  46.1  34.4  36.2   69.0   6.7          E8   1.0  2016   False  \n",
       "6  47.2  48.1  36.0  30.7   65.5  -0.3          F4  10.0  2016   False  \n",
       "\n",
       "[5 rows x 25 columns]"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df1 = df.loc[df['POSTSEASON'].str.contains('F4|S16|E8', na=False)]\n",
    "df1.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "S16    32\n",
       "E8     16\n",
       "F4      8\n",
       "Name: POSTSEASON, dtype: int64"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df1['POSTSEASON'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "32 teams made it into the Sweet Sixteen, 16 into the Elite Eight, and 8 made it into the Final Four over 5 seasons. \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Lets plot some columns to underestand the data better:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Retrieving notices: ...working... done\n",
      "Collecting package metadata (current_repodata.json): done\n",
      "Solving environment: done\n",
      "\n",
      "## Package Plan ##\n",
      "\n",
      "  environment location: /home/jupyterlab/conda/envs/python\n",
      "\n",
      "  added / updated specs:\n",
      "    - seaborn\n",
      "\n",
      "\n",
      "The following packages will be downloaded:\n",
      "\n",
      "    package                    |            build\n",
      "    ---------------------------|-----------------\n",
      "    ca-certificates-2023.08.22 |       h06a4308_0         130 KB  anaconda\n",
      "    certifi-2020.6.20          |     pyhd3eb1b0_3         159 KB  anaconda\n",
      "    openssl-1.1.1w             |       h7f8727e_0         3.8 MB  anaconda\n",
      "    seaborn-0.12.2             |   py37h06a4308_0         487 KB  anaconda\n",
      "    ------------------------------------------------------------\n",
      "                                           Total:         4.6 MB\n",
      "\n",
      "The following NEW packages will be INSTALLED:\n",
      "\n",
      "  seaborn            anaconda/linux-64::seaborn-0.12.2-py37h06a4308_0 \n",
      "\n",
      "The following packages will be UPDATED:\n",
      "\n",
      "  ca-certificates    conda-forge::ca-certificates-2023.5.7~ --> anaconda::ca-certificates-2023.08.22-h06a4308_0 \n",
      "  openssl            conda-forge::openssl-1.1.1t-h0b41bf4_0 --> anaconda::openssl-1.1.1w-h7f8727e_0 \n",
      "\n",
      "The following packages will be SUPERSEDED by a higher-priority channel:\n",
      "\n",
      "  certifi            conda-forge::certifi-2023.5.7-pyhd8ed~ --> anaconda::certifi-2020.6.20-pyhd3eb1b0_3 \n",
      "\n",
      "\n",
      "\n",
      "Downloading and Extracting Packages\n",
      "ca-certificates-2023 | 130 KB    |                                       |   0% \n",
      "seaborn-0.12.2       | 487 KB    |                                       |   0% \u001b[A\n",
      "\n",
      "openssl-1.1.1w       | 3.8 MB    |                                       |   0% \u001b[A\u001b[A\n",
      "\n",
      "\n",
      "certifi-2020.6.20    | 159 KB    |                                       |   0% \u001b[A\u001b[A\u001b[A\n",
      "ca-certificates-2023 | 130 KB    | ####5                                 |  12% \u001b[A\n",
      "\n",
      "openssl-1.1.1w       | 3.8 MB    | 1                                     |   0% \u001b[A\u001b[A\n",
      "\n",
      "\n",
      "certifi-2020.6.20    | 159 KB    | ###7                                  |  10% \u001b[A\u001b[A\u001b[A\n",
      "\n",
      "ca-certificates-2023 | 130 KB    | ##################################### | 100% \u001b[A\u001b[A\n",
      "\n",
      "\n",
      "certifi-2020.6.20    | 159 KB    | ##################################### | 100% \u001b[A\u001b[A\u001b[A\n",
      "\n",
      "\n",
      "certifi-2020.6.20    | 159 KB    | ##################################### | 100% \u001b[A\u001b[A\u001b[A\n",
      "seaborn-0.12.2       | 487 KB    | ##################################### | 100% \u001b[A\n",
      "seaborn-0.12.2       | 487 KB    | ##################################### | 100% \u001b[A\n",
      "\n",
      "openssl-1.1.1w       | 3.8 MB    | ##################################### | 100% \u001b[A\u001b[A\n",
      "\n",
      "                                                                                \u001b[A\u001b[A\n",
      "                                                                                \u001b[A\n",
      "\n",
      "                                                                                \u001b[A\u001b[A\n",
      "\n",
      "\n",
      "                                                                                \u001b[A\u001b[A\u001b[A\n",
      "Preparing transaction: done\n",
      "Verifying transaction: done\n",
      "Executing transaction: done\n"
     ]
    }
   ],
   "source": [
    "# notice: installing seaborn might takes a few minutes\n",
    "!conda install -c anaconda seaborn -y"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAmEAAAEiCAYAAACx0QUUAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMywgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/NK7nSAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAss0lEQVR4nO3deXQUdb7+8ScLWTskkiAESVgEwiYIEQFBFhcCguIMF9EDIuhFvKIkooiobCpEwVHUEbwwkKCI4SIDIgqKo2EQAsMWwSGE3QC/xNhsSQgJWer3h0OPbRKGTrq7srxf59Q5U9VV9f30l56PT6qruz0MwzAEAAAAt/I0uwAAAIC6iBAGAABgAkIYAACACQhhAAAAJiCEAQAAmIAQBgAAYAJCGAAAgAkIYQAAACYghAEAAJiAEFZHJSYmKiQkpMrn6devn+Li4qp8nupq5syZuvnmm80uA6iT6FOo7QhhddSIESN06NAhs8twi379+snDw6PMUlxcbHZpAK6iLvSpEydOlNuffrvMnDnT7DLhIt5mFwBz+Pv7y9/f3+wy3GbcuHF65ZVX7LZ5e/PyB6qzutCnIiIilJmZaVt/8803tXHjRn3zzTe2bRaLxfa/DcNQSUkJ/auW4EpYLfH5558rJCREpaWlkqTU1FR5eHho8uTJtn3Gjx+vhx56SFLZy/xX3nb76KOP1Lx5cwUHB+vBBx9Ubm6ubZ+LFy9q9OjRslgsCg8P15/+9KcydVy+fFnPP/+8brjhBgUGBqp79+5KTk6WJBUUFKhDhw56/PHHbfsfP35cwcHBWrx4sTOno4yAgAA1btzYbpGkKVOmqE2bNgoICFDLli01bdo0FRUVVXie5ORk3XrrrQoMDFRISIh69eqln376yfb4559/rujoaPn5+ally5aaNWsWV9yAf6FPleXl5WXXlywWi7y9vW3rBw8eVFBQkL766ivdcsst8vX11ZYtWzRmzBjdf//9dueKi4tTv379bOuGYWju3Llq2bKl/P391blzZ3366adOfw6oPEJYLdGnTx/l5uZq7969kqTNmzcrLCxMmzdvtu2TnJysvn37VniOo0ePau3atVq/fr3Wr1+vzZs36/XXX7c9PnnyZH333Xdas2aNvv76ayUnJ2v37t125xg7dqy2bt2qpKQk7du3T8OHD9fAgQN1+PBh+fn56eOPP9ayZcu0du1alZSU6OGHH1b//v01bty4CusaNGiQLBbLVZfKCgoKUmJiog4cOKB33nlHixcv1ttvv13uvsXFxbr//vvVt29f7du3TykpKXr88cfl4eEhSfrqq680atQoTZw4UQcOHND//u//KjExUbNnz650fUBtQp+qvOeff17x8fFKS0tTp06drumYl19+WQkJCVq4cKH++c9/6plnntGoUaPs5hsmM1BrdO3a1XjzzTcNwzCM+++/35g9e7bh4+Nj5OTkGJmZmYYkIy0tzTAMw0hISDCCg4Ntx86YMcMICAgwcnJybNsmT55sdO/e3TAMw8jNzTV8fHyMpKQk2+Nnzpwx/P39jdjYWMMwDOPIkSOGh4eHcfr0abu67rzzTmPq1Km29blz5xphYWHG008/bTRu3Nj45Zdfrvq8Tp06ZRw+fPiqy9X07dvXqFevnhEYGGhbJk2aVO6+c+fONaKjo+3mpXPnzrbnK8lITk4u99jbb7/dmDNnjt22jz76yAgPD79qfUBdQp+6ut/2HMMwjO+++86QZKxdu9Zuv0ceecQYOnSo3bbY2Fijb9++hmEYRl5enuHn52ds27bNbp/HHnvMeOihh66pFrgebyrXIv369VNycrImTZqkLVu26LXXXtPq1av1/fff6/z582rUqJHatm1b4fHNmzdXUFCQbT08PFzZ2dmSfv3r8/Lly+rZs6ft8QYNGigqKsq2vmfPHhmGoTZt2tidt7CwUKGhobb1Z599Vp999pnee+89bdiwQWFhYVd9XjfccMO1TcBVjBw5Ui+99JJt/cpbHJ9++qnmz5+vI0eOKC8vT8XFxapfv36552jQoIHGjBmjmJgY3X333brrrrv0wAMPKDw8XJK0e/du7dy50+7KV0lJiQoKCpSfn6+AgIAqPw+gpqNPVc4tt9zi0P4HDhxQQUGB7r77brvtly9fVpcuXZxZGqqAEFaL9OvXT0uWLNEPP/wgT09PtW/fXn379tXmzZt17ty5q17il6R69erZrXt4eNju3TAM4z+OX1paKi8vL+3evVteXl52j/32Unx2drbS09Pl5eWlw4cPa+DAgVc976BBg7Rly5ar7pOXl3fVx4ODg9WqVSu7bdu3b9eDDz6oWbNmKSYmRsHBwUpKSir3HpIrEhISNHHiRG3cuFErV67Uyy+/rE2bNqlHjx4qLS3VrFmz9Mc//rHMcX5+fletD6gr6FOVExgYaLfu6elZ5vn+9n7WK3PyxRdflAmIvr6+la4DzkUIq0Wu3G8xf/589e3bVx4eHurbt6/i4+N17tw5xcbGVvrcrVq1Ur169bR9+3ZFRkZKks6dO6dDhw7ZmmaXLl1UUlKi7Oxs3X777RWe69FHH1XHjh01btw4PfbYY7rzzjvVvn37Cvf/y1/+okuXLlW69ops3bpVzZo1s7tC9tub7CvSpUsXdenSRVOnTlXPnj21YsUK9ejRQ127dlV6enqZsAfg3+hTztGwYUP9+OOPdttSU1NtIbV9+/by9fVVRkbGfwy2MA8hrBYJDg7WzTffrOXLl+udd96R9GvDGz58uIqKiuw+NeMoi8Wixx57TJMnT1ZoaKgaNWqkl156SZ6e//5sR5s2bTRy5EiNHj1af/rTn9SlSxdZrVZ9++23uummm3TPPffo/fffV0pKivbt26eIiAht2LBBI0eO1I4dO+Tj41Pu2K66zN+qVStlZGQoKSlJ3bp10xdffKE1a9ZUuP/x48e1aNEi3XfffWrSpInS09N16NAhjR49WpI0ffp0DRkyRBERERo+fLg8PT21b98+7d+/X6+99ppLngNQ09CnnOOOO+7QvHnz9OGHH6pnz55avny5fvzxR9tbjUFBQXruuef0zDPPqLS0VL1791ZOTo62bdsmi8WiRx55xK31onx8OrKW6d+/v0pKSmyN7LrrrlP79u3VsGFDtWvXrkrnnjdvnvr06aP77rtPd911l3r37q3o6Gi7fRISEjR69Gg9++yzioqK0n333acdO3YoIiJCBw8e1OTJk7VgwQJFRERIkt5//32dP39e06ZNq1JtlTF06FA988wzeuqpp3TzzTdr27ZtV60jICBABw8e1LBhw9SmTRs9/vjjeuqppzR+/HhJUkxMjNavX69NmzapW7du6tGjh9566y01a9bMXU8JqBHoU1UXExOjadOm6fnnn1e3bt2Um5tr+4PwildffVXTp09XfHy82rVrp5iYGH3++edq0aKFSVXj9zyMa3kTHQAAAE7FlTAAAAATEMIAAABMQAgDAAAwASEMAADABIQwAAAAExDCAAAATOD2EGYYhnJycq7p5yUAwF3oTQDcze0hLDc3V8HBwcrNzXX30ABQIXoTAHfj7UgAAAATEMIAAABMQAgDAAAwASEMAADABIQwAAAA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IWVmZjr9tJUOYU899ZT27dun77///qr7TZ06VZMmTbKt5+TkKCIiorLDupzValVBfoG6xHWWpanF5eNl78lW+orDbhsv71Se9s7/QceOHSOE1VCTLfV1h5+fy8f5tqBA8/Jy9G5IA7X2rtJF8//ocHGxJp4/K6vV6rYQVtN6E+Copv1HK6RVN5ePc/7ITp367kOXj3NF8cUiyZDb/7t5/vx5p5+7Up316aef1rp16/T3v/9dTZs2veq+vr6+bns7w5ksTS0KuTHY5ePkncpz63io+SK9vXSTj4/Lx7lyCb61t7dbxnO3mtqbgGvlG9JYgeGtXT7OJetJl49Rntrw302HQphhGHr66ae1Zs0aJScnq0WLFq6qCwAAoFZzKIRNmDBBK1as0GeffaagoCBlZWVJkoKDg+Xv7++SAgEAAGojh74nbOHChbpw4YL69eun8PBw27Jy5UpX1QcAAFArOfx2JAAAAKqO344EAAAwASEMAADABIQwAAAAExDCAAAATEAIAwAAMAEhDAAAwASEMAAAABMQwgAAAExACAMAADABIQwAAMAEhDAAAAATEMIAAABMQAgDAAAwASEMAADABIQwAAAAExDCAAAATEAIAwAAMIG32QVci5SUFB07dswtYx0/ftwt45ht69atLh+jrsylu+0sLKxV4wBmycjIkNVqdfk4aWlpLh8DNVO1D2EpKSnq1buXjFLD7FJqhYJzhZKHtHDhQi1cuNDscuCAK/92H17K14eX8t0zqIeUVnRZN/n4uGc8wE0yMjIU1S5KBfkFZpeCGsIVgb3ah7Bjx47JKDXUJa6zLE0tLh8ve0+20lccdvk4Zim+WCQZcst81va5dDd3/ttJUt6pPO2d/4OySkpcPhbgblarVQX5BfRCXLPc3Fynn7Pah7ArLE0tCrkx2OXj5J3Kc/kY1YE75rOuzKW7uev/C0BdQC+EmbgxHwAAwASEMAAAABMQwgAAAExACAMAADABIQwAAMAEhDAAAAATEMIAAABMQAgDAAAwASEMAADABIQwAAAAExDCAAAATEAIAwAAMAEhDAAAwASEMAAAABMQwgAAAExACAMAADABIQwAAMAElQphCxYsUIsWLeTn56fo6Ght2bLF2XUBAADUag6HsJUrVyouLk4vvfSS9u7dq9tvv12DBg1SRkaGK+oDAAColRwOYW+99ZYee+wx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TUBvcj56k/PRnxxHCPud999/32jWrJnh4+NjdO3a1di8ebPtsUceecTo27ev3f7vvvuu0b59e8Pf398IDw83Ro4caZw6dcr2eFxcnBEZGWn4+PgYDRs2NAYMGGBs27bNXU+nWnBkTg8cOGDcfPPNhr+/v1G/fn1j6NChxsGDB8ucc9WqVUZUVJRRr149o23btsbq1avd8VSqBWfPJ6/RmoHe5Hz0JuejPznGwzAquDYNAAAAl+GeMAAAABMQwgAAAExACAMAADABIQwAAMAEhDAAAAATEMIAAABMQAgDAAAwASEMAADABIQwAAAAExDCUK4xY8bIw8PDtoSGhmrgwIHat29fmX0ff/xx2w+v/t7MmTNt5/D09FSTJk00cuRInTx5UidOnLAbo7xl5syZtv1SU1PLnL9fv36Ki4srs33FihXy8vLSE088Ue7zy8nJ0bRp09ShQwf5+/srNDRU3bp109y5c3Xu3DmH5wuAe9CbUJsQwlChgQMHKjMzU5mZmfrb3/4mb29vDRkyxG6f/Px8rVy5UpMnT9aSJUvKPU+HDh2UmZmpU6dOaeXKldq/f78eeOABRURE2M6fmZmpZ5991rbvleW5556rVO1Lly7V888/r6SkJOXn59s9dvbsWfXo0UMJCQl67rnntGPHDm3dulUzZsxQamqqVqxYUakxAbgHvQm1hbfZBaD68vX1VePGjSVJjRs31pQpU9SnTx/98ssvatiwoSRp1apVat++vaZOnarw8HCdOHFCzZs3tzuPt7e37TxNmjTRuHHjNHHiRF28eNG2XZIsFovdvldYrVaH6j5x4oS2bdum1atX67vvvtOnn36q0aNH2x5/8cUXlZGRofT0dN1www227W3bttWQIUPEz6kC1Ru9CbUFV8JwTfLy8vTxxx+rVatWCg0NtW1fsmSJRo0apeDgYN1zzz1KSEi46nmysrL017/+VV5eXvLy8nJJrUuXLtXgwYMVHBysUaNG2f0VXFpaqpUrV2rUqFF2Te63PDw8XFIXAOejN6EmI4ShQuvXr5fFYpHFYlFQUJDWrVunlStXytPz15fN4cOHtX37do0YMUKSNGrUKCUkJKi0tNTuPPv375fFYlFAQIDCw8OVnJysCRMmKDAw0KF6brvtNls9V5YtW7bY7VNaWqrExESNGjVKkvTggw8qJSVFR44ckST98ssvOn/+vKKiouyOi46Otp3zoYcecqguAO5Fb0JtQQhDhfr376/U1FSlpqZqx44dGjBggAYNGqSffvpJ0q9/acbExCgsLEySdM899+jixYv65ptv7M4TFRWl1NRU7dy5U7Nnz9bNN9+s2bNnO1zPypUrbfVcWW655Ra7fb7++mtdvHhRgwYNkiSFhYVpwIABWrp0qd1+v/+Lcs2aNUpNTVVMTIwuXbrkcG0A3IfehNqCe8JQocDAQLVq1cq2Hh0dreDgYC1evFizZs3Shx9+qKysLHl7//tlVFJSoiVLlmjAgAG2bT4+PrbzdOjQQYcPH9b//M//6KOPPnKonoiICLt6JMnf399ufenSpTp79qwCAgJs20pLS7V37169+uqratiwoUJCQnTw4EG74yIjIyVJQUFBOn/+vEN1AXAvehNqC66E4Zpd+Sj3pUuX9OWXXyo3N1d79+61++tv1apVWrt2rc6cOVPheaZNm6ZPPvlEe/bscWp9Z86c0WeffaakpKQyf5Xm5eVpw4YN8vT01AMPPKDly5fr9OnTTh0fgDnoTaipCGGoUGFhobKyspSVlaW0tDQ9/fTTysvL07333qslS5Zo8ODB6ty5szp27Ghbhg0bpoYNG2r58uUVnrdly5YaOnSopk+f7tR6P/roI4WGhmr48OF2NXXq1ElDhgyx3QQ7Z84c3XDDDerevbuWLl2qffv26ejRo1qzZo1SUlJcdlMuAOegN6G24O1IVGjjxo0KDw+X9Oul8LZt22rVqlVq166dvvjii3K/s8bDw0N//OMftWTJEsXGxlZ47meffVa9evXSjh071L17d6fUu3TpUv3hD3+w3Zz7W8OGDdOIESP0888/q1GjRvrHP/6hN954Q/PmzdPx48fl6emp1q1ba8SIEeV+wSKA6oPehNrCw+CLRwAAANyOtyMBAABMQAgDAAAwASEMAADABIQwAAAAExDCAAAATEAIAwAAMAEhDAAAwASEMAAAABMQwgAAAExACAMAADABIQwAAMAEhDAAAAAT/H9dZPx88nus9QAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 1800x300 with 2 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "import seaborn as sns\n",
    "\n",
    "bins = np.linspace(df1.BARTHAG.min(), df1.BARTHAG.max(), 10)\n",
    "g = sns.FacetGrid(df1, col=\"windex\", hue=\"POSTSEASON\", palette=\"Set1\", col_wrap=6)\n",
    "g.map(plt.hist, 'BARTHAG', bins=bins, ec=\"k\")\n",
    "\n",
    "g.axes[-1].legend()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAk4AAAEiCAYAAAAPh11JAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMywgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/NK7nSAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAodUlEQVR4nO3deXQUdb7+8aezdchiMIlCkARBIOwxgCJXJSAgoALei4zMZXUQmTOIARQdQDZF42FRnIuiOJjgKAeug6CiMsJoWARGJKwSwiLY4k2MYUtCSMhSvz889M+ehFid9Jbk/TqnzrGqq+v7qZqaTx6qqrsthmEYAgAAwG/y83YBAAAAdQXBCQAAwCSCEwAAgEkEJwAAAJMITgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwakBSEtLU+PGjWu9nd69e2vKlCm13o6vmjdvnm699VZvlwHUK/Qf1DcEpwbg4Ycf1rFjx7xdhkf07t1bFoul0lRWVubt0oAGqSH0n9OnT1fZd349zZs3z9tlwkUCvF0A3K9Ro0Zq1KiRt8vwmAkTJui5555zWBYQwKkOeEND6D+xsbHKzs62zy9evFibNm3Sli1b7MvCwsLs/20YhsrLy+lLdRRXnOqgjz/+WI0bN1ZFRYUkaf/+/bJYLJo+fbp9nYkTJ+r3v/+9pMqXyq/ekvrb3/6mm2++WRERERoxYoQKCgrs61y6dEljxoxRWFiYYmJitGTJkkp1XLlyRU8//bRuuukmhYaGqkePHkpPT5ckFRcXq2PHjnrsscfs6586dUoRERF66623XHk4KgkJCVHTpk0dJkl65pln1LZtW4WEhKhVq1aaPXu2SktLr7md9PR03X777QoNDVXjxo1155136vvvv7e//vHHH6tbt24KDg5Wq1atNH/+fK5sod6j/1Tm7+/v0G/CwsIUEBBgnz969KjCw8P1j3/8Q927d5fVatX27ds1btw4Pfjggw7bmjJlinr37m2fNwxDCxcuVKtWrdSoUSMlJCTo73//u8v3AeYRnOqgXr16qaCgQPv27ZMkbd26VdHR0dq6dat9nfT0dCUlJV1zGydPntSGDRu0ceNGbdy4UVu3btVLL71kf3369On68ssvtX79en3++edKT0/X3r17HbbxyCOP6KuvvtKaNWt08OBBDR8+XAMHDtTx48cVHBys9957T6tWrdKGDRtUXl6u0aNHq0+fPpowYcI16xo0aJDCwsKqnWoqPDxcaWlpOnLkiF599VW99dZbeuWVV6pct6ysTA8++KCSkpJ08OBB7dq1S4899pgsFosk6R//+IdGjRqlJ554QkeOHNGbb76ptLQ0vfDCCzWuD6gL6D819/TTTyslJUWZmZnq0qWLqfc8++yzSk1N1fLly/Xtt99q6tSpGjVqlMPxhocZqJO6du1qLF682DAMw3jwwQeNF154wQgKCjLy8/ON7OxsQ5KRmZlpGIZhpKamGhEREfb3zp071wgJCTHy8/Pty6ZPn2706NHDMAzDKCgoMIKCgow1a9bYXz979qzRqFEjIzk52TAMwzhx4oRhsViMH3/80aGuvn37GjNmzLDPL1y40IiOjjYmT55sNG3a1Pj555+r3a8zZ84Yx48fr3aqTlJSkhEYGGiEhobap2nTplW57sKFC41u3bo5HJeEhAT7/koy0tPTq3zv3Xffbbz44osOy/72t78ZMTEx1dYH1Af0n+r9upcYhmF8+eWXhiRjw4YNDuuNHTvWGDp0qMOy5ORkIykpyTAMwygsLDSCg4ONnTt3Oqwzfvx44/e//72pWuB63GCto3r37q309HRNmzZN27dv14IFC7Ru3Trt2LFDFy5cUJMmTdSuXbtrvv/mm29WeHi4fT4mJka5ubmSfvnX4JUrV9SzZ0/765GRkYqPj7fPZ2RkyDAMtW3b1mG7JSUlioqKss8/+eST+vDDD/U///M/+uyzzxQdHV3tft10003mDkA1Ro4cqVmzZtnnr94m+Pvf/66lS5fqxIkTKiwsVFlZma677roqtxEZGalx48ZpwIAB6t+/v/r166ff/e53iomJkSTt3btXe/bscbjCVF5eruLiYhUVFSkkJKTW+wH4KvpPzXTv3t2p9Y8cOaLi4mL179/fYfmVK1eUmJjoytLgBIJTHdW7d2+tXLlSBw4ckJ+fnzp06KCkpCRt3bpV58+fr/YyuSQFBgY6zFssFvszC4Zh/Ob4FRUV8vf31969e+Xv7+/w2q8vZ+fm5iorK0v+/v46fvy4Bg4cWO12Bw0apO3bt1e7TmFhYbWvR0REqHXr1g7Ldu/erREjRmj+/PkaMGCAIiIitGbNmiqfnbgqNTVVTzzxhDZt2qS1a9fq2Wef1ebNm3XHHXeooqJC8+fP13/9139Vel9wcHC19QF1Hf2nZkJDQx3m/fz8Ku3vr5+7vHpMPvnkk0qhzmq11rgO1A7BqY66+pzB0qVLlZSUJIvFoqSkJKWkpOj8+fNKTk6u8bZbt26twMBA7d69W3FxcZKk8+fP69ixY/aGmJiYqPLycuXm5uruu+++5rb+8Ic/qFOnTpowYYLGjx+vvn37qkOHDtdc/69//asuX75c49qv5auvvlKLFi0crkT9+kHva0lMTFRiYqJmzJihnj17avXq1brjjjvUtWtXZWVlVQpoQENA/3GNG264QYcPH3ZYtn//fnuw7NChg6xWq2w222+GUXgOwamOioiI0K233qp3331Xr776qqRfmtnw4cNVWlrq8KkMZ4WFhWn8+PGaPn26oqKi1KRJE82aNUt+fv//swRt27bVyJEjNWbMGC1ZskSJiYnKy8vTF198oc6dO+u+++7Ta6+9pl27dungwYOKjY3VZ599ppEjR+pf//qXgoKCqhzbXZfKW7duLZvNpjVr1ui2227TJ598ovXr119z/VOnTmnFihUaMmSImjVrpqysLB07dkxjxoyRJM2ZM0cPPPCAYmNjNXz4cPn5+engwYM6dOiQFixY4JZ9AHwF/cc17rnnHi1atEjvvPOOevbsqXfffVeHDx+234YLDw/XU089palTp6qiokJ33XWX8vPztXPnToWFhWns2LEerRe/4FN1dVifPn1UXl5ub1LXX3+9OnTooBtuuEHt27ev1bYXLVqkXr16aciQIerXr5/uuusudevWzWGd1NRUjRkzRk8++aTi4+M1ZMgQ/etf/1JsbKyOHj2q6dOn6/XXX1dsbKwk6bXXXtOFCxc0e/bsWtVWE0OHDtXUqVP1+OOP69Zbb9XOnTurrSMkJERHjx7VsGHD1LZtWz322GN6/PHHNXHiREnSgAEDtHHjRm3evFm33Xab7rjjDr388stq0aKFp3YJ8Cr6T+0NGDBAs2fP1tNPP63bbrtNBQUF9n+cXfX8889rzpw5SklJUfv27TVgwAB9/PHHatmypZeqhsUwc0MZAAAAXHECAAAwi+AEAABgEsEJAADAJIITAACASQQnAAAAkwhOAAAAJnk8OBmGofz8fFNfqw8AtUXPAeBKHg9OBQUFioiIUEFBgaeHBtAA0XMAuBK36gAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMCkgNq8OSUlRTNnzlRycrKWLl3qopIAAIBZFRUVunLlirfL8GmBgYHy9/d3ybZqHJz27NmjFStWqEuXLi4pBAAAOOfKlSs6deqUKioqvF2Kz2vcuLGaNm0qi8VSq+3UKDgVFhZq5MiReuutt7RgwYJaFQAAAJxnGIays7Pl7++v2NhY+fnx9E1VDMNQUVGRcnNzJUkxMTG12l6NgtOkSZN0//33q1+/fr8ZnEpKSlRSUmKfz8/Pr8mQQCU2m015eXkeGaukpERWq7VejRUdHa24uDi3j+Np9Bw0FGVlZSoqKlKzZs0UEhLi7XJ8WqNGjSRJubm5uvHGG2t1287p4LRmzRplZGRoz549ptZPSUnR/PnznS4MqI7NZlN8+3gVFxV7ZDyLn0VGhWd+ssNTYwWHBCsrM6vehSd6DhqK8vJySVJQUJCXK6kbrobL0tJSzwWnH374QcnJyfr8888VHBxs6j0zZszQtGnT7PP5+fmKjY11rkrg3+Tl5am4qFiJUxIU1jzMrWPlZuQqa/XxejVW4ZlC7Vt6QHl5efUuONFz0NDU9pmdhsJVx8mp4LR3717l5uaqW7du9mXl5eXatm2bli1bppKSkkopzmq1euwWBxqesOZhanxLhFvHKDxTWC/Hqq/oOQDcyang1LdvXx06dMhh2SOPPKJ27drpmWeecdlH/QAAQM148vlPqf4+L3ktTgWn8PBwderUyWFZaGiooqKiKi0HAACeZbPZ1D4+XkXFnnn+U5JCgoOVmWX+eclx48Zp1apVlZYPGDBAmzZtUk5OjqZPn67NmzeroKBA8fHxmjlzph566CFXl14jtfoCTAAA4Dvy8vJUVFysvzSOVJsA9/+JP15WpicunHP6ecmBAwcqNTXVYdnVW+yjR4/WxYsX9dFHHyk6OlqrV6/Www8/rG+++UaJiYkurb8man1U09PTXVAGAABwlTYBAersw5+2s1qtatq0aZWv7dq1S8uXL9ftt98uSXr22Wf1yiuvKCMjwyeCE9+WBQAAfMZdd92ltWvX6ty5c6qoqNCaNWtUUlKi3r17e7s0SQQnAADgYRs3blRYWJjD9Pzzz0uS1q5dq7KyMkVFRclqtWrixIlav369brnlFi9X/QuecQIAAB7Vp08fLV++3GFZZGSkpF9uzZ0/f15btmxRdHS0NmzYoOHDh2v79u3q3LmzN8p1QHACAAAeFRoaqtatW1dafvLkSS1btkyHDx9Wx44dJUkJCQnavn27XnvtNb3xxhueLrUSbtUBAACfUFRUJEmVfrDY399fFRUV3iipEq44AQAAjyopKVFOTo7DsoCAALVr106tW7fWxIkTtXjxYkVFRWnDhg3avHmzNm7c6KVqHRGcAACoZ46Xlfn0OJs2bVJMTIzDsvj4eB09elSffvqp/vznP2vw4MEqLCxU69attWrVKt13332uKLnWCE4AANQT0dHRCgkO1hMXznlszJDgYEVHR5tePy0tTWlpadd8vU2bNlq3bp0LKnMPghMAAPVEXFycMrOy+K06NyI4AQBQj8TFxTWoIONpfKoOAADAJIITAACASQQnAAAAkwhOAAAAJhGcAAAATCI4AQAAmERwAgAAMInvcQIAoB6x2Wx8AaYbEZwAAKgnbDab4tu1V/HlIo+NGdwoRFlHM02Hp3HjxmnVqlWVlh8/flytW7e2z6ekpGjmzJlKTk7W0qVLXVVurRGcAACoJ/Ly8lR8uUi3/OfTahQd6/bxLuf9oJPrFyovL8+pq04DBw5Uamqqw7IbbrjB/t979uzRihUr1KVLF5fV6ioEJwAA6plG0bEKjWnj7TKuyWq1qmnTplW+VlhYqJEjR+qtt97SggULPFzZb+PhcAAA4DMmTZqk+++/X/369fN2KVXiihMAAPCojRs3KiwszD4/aNAgvf/++1qzZo0yMjK0Z88eL1ZXPYITAADwqD59+mj58uX2+dDQUP3www9KTk7W559/ruDgYC9WVz2CEwAA8KjQ0FCHT9BJ0oYNG5Sbm6tu3brZl5WXl2vbtm1atmyZSkpK5O/v7+lSKyE4AQAAr+vbt68OHTrksOyRRx5Ru3bt9Mwzz/hEaJIITgAAwAeEh4erU6dODstCQ0MVFRVVabk3EZwAAKhnLuf9UK/G8SUEJwAA6ono6GgFNwrRyfULPTZmcKMQRUdHm14/LS3N9Lrp6enOF+RmBCcAAOqJuLg4ZR3N5Lfq3IjgBABAPRIXF9eggoyn8c3hAAAAJhGcAAAATCI4AQAAmERwAgAAMIngBAAAYBLBCQAAwCSCEwAAgElOfY/T8uXLtXz5cp0+fVqS1LFjR82ZM0eDBg1yR20AAMBJNpuNL8B0I6eCU/PmzfXSSy+pdevWkqRVq1Zp6NCh2rdvnzp27OiWAgEAgDk2m03x7eNVXFTssTGDQ4KVlZnlVHjKzc3V7Nmz9dlnn+mnn37S9ddfr4SEBM2bN089e/bUihUrtHr1amVkZKigoEDnz59X48aNK23nk08+0XPPPaeDBw8qNDRUvXr10gcffODCvavMqeA0ePBgh/kXXnhBy5cv1+7duwlOAAB4WV5enoqLipU4JUFhzcPcPl7hmULtW3pAeXl5TgWnYcOGqbS0VKtWrVKrVq30008/6Z///KfOnTsnSSoqKtLAgQM1cOBAzZgxo8ptrFu3ThMmTNCLL76oe+65R4Zh6NChQy7Zr+rU+CdXysvL9f777+vSpUvq2bOnK2sCAAC1ENY8TI1vifB2GVW6cOGCduzYofT0dCUlJUmSWrRoodtvv92+zpQpUyRd+0d+y8rKlJycrEWLFmn8+PH25fHx8W6r+yqng9OhQ4fUs2dPFRcXKywsTOvXr1eHDh2uuX5JSYlKSkrs8/n5+TWrFHWCp+6tZ2Zmun2MhsBTx9GTz0DQc+BOnupx9fm5obCwMIWFhWnDhg264447ZLVand5GRkaGfvzxR/n5+SkxMVE5OTm69dZbtXjxYrffAXM6OMXHx2v//v26cOGC1q1bp7Fjx2rr1q3XDE8pKSmaP39+rQuF7/PGvXXUzqhRozwyTkhwsDKznHsGoqboOXAXT/a4mjw3VFcEBAQoLS1NEyZM0BtvvKGuXbsqKSlJI0aMUJcuXUxt47vvvpMkzZs3Ty+//LJuvvlmLVmyRElJSTp27JgiIyPdV7+zbwgKCrI/HN69e3ft2bNHr776qt58880q158xY4amTZtmn8/Pz1dsbGwNy4Uv8+S99dyMXGWtPu7WMRqC6WHX6Z7gYLeOcbysTE9cOOf0MxA1Rc+Bu3iqx9X0uaG6ZNiwYbr//vu1fft27dq1S5s2bdLChQv117/+VePGjfvN91dUVEiSZs2apWHDhkmSUlNT1bx5c73//vuaOHGi22qv8TNOVxmG4XBZ/N9ZrdYaXYZD3eWJe+uFZwrduv2GIi7AX52DgrxdhkvRc+Buvvz8UF0SHBys/v37q3///pozZ44effRRzZ0711RwiomJkSSHu11Wq1WtWrWSzWZzV8mSnPwCzJkzZ2r79u06ffq0Dh06pFmzZik9PV0jR450V30AAKAB6NChgy5dumRq3W7duslqtSorK8u+rLS0VKdPn1aLFi3cVaIkJ684/fTTTxo9erSys7MVERGhLl26aNOmTerfv7+76gMAAPXI2bNnNXz4cP3hD39Qly5dFB4erm+++UYLFy7U0KFDJUk5OTnKycnRiRMnJP3ywbTw8HDFxcUpMjJS1113nf74xz9q7ty5io2NVYsWLbRo0SJJ0vDhw91av1PBaeXKle6qAwAAuIinHmeoyThhYWHq0aOHXnnlFZ08eVKlpaWKjY3VhAkTNHPmTEnSG2+84fAhj169ekn65Tmmq7fyFi1apICAAI0ePVqXL19Wjx499MUXX+j666+v/Y5Vo9bPOAEAAN8QHR2t4JBg7Vt6wGNjBocEKzo62vT6VqtVKSkpSklJueY68+bN07x586rdTmBgoBYvXqzFixebHtsVCE4AANQTcXFxysrM4rfq3IjgBABAPRIXF9eggoynOfWpOgAAgIaM4AQAAGASwQkAAMAkghMAAHWYYRjeLqFOuPozLbXFw+EAANRBgYGBslgs+vnnn3XDDTfIYrF4uySfZBiGrly5op9//ll+fn4KquXPTBGcAACog/z9/dW8eXOdOXNGp0+f9nY5Pi8kJERxcXHy86vdzTaCEwAAdVRYWJjatGmj0tJSb5fi0/z9/RUQEOCSq3IEJwAA6jB/f3/5+/t7u4wGg4fDAQAATCI4AQAAmERwAgAAMIngBAAAYBLBCQAAwCSCEwAAgEkEJwAAAJMITgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMAkghMAAIBJBCcAAACTCE4AAAAmEZwAAABMIjgBAACYRHACAAAwieAEAABgEsEJAADAJIITAACASQQnAAAAkwhOAAAAJhGcAAAATCI4AQAAmERwAgAAMMmp4JSSkqLbbrtN4eHhuvHGG/Xggw8qKyvLXbUBAAD4FKeC09atWzVp0iTt3r1bmzdvVllZme69915dunTJXfUBAAD4jABnVt60aZPDfGpqqm688Ubt3btXvXr1cmlhAAAAvqZWzzhdvHhRkhQZGemSYgAAAHyZU1ecfs0wDE2bNk133XWXOnXqdM31SkpKVFJSYp/Pz8+v6ZAeYbPZlJeX5/ZxoqOjFRcX5/ZxJGnXrl367rvv3D7OqVOn3D4GXOuHsjIdunLFrWOcKCt16/b/XV3rOcC1ZGZmemSckpISWa3WejeWu/7O1jg4Pf744zp48KB27NhR7XopKSmaP39+TYfxKJvNpvj28SouKnb7WMEhwcrKzHJ7eNq1a5fuvOtOGRWGW8dB3VJ8vkSySAsLC7SwsMD9A1qk7Oxs94+jutVzgOqMGjXKI+P4SarwyEiSxc/isb9H7vo7W6PgNHnyZH300Ufatm2bmjdvXu26M2bM0LRp0+zz+fn5io2NrcmwbpeXl6fiomIlTklQWPMwt41TeKZQ+5YeUF5entuD03fffSejwnD7PklSbkauslYfd+sYcI2yS6WSIY+cF1fP9wsXLrh1nKvqUs8BqjM97DrdExzs1jG+KC7WosJ8/aVxpNoE1PhailNjebLvuOPvrFNHyTAMTZ48WevXr1d6erpatmz5m++xWq0euyznKmHNw9T4lghvl+FSntinwjOFbt0+XK8+nut1secAVYkL8FfnoCC3jnH1VnqbgACPjVXX+45TwWnSpElavXq1PvzwQ4WHhysnJ0eSFBERoUaNGrmlQAAAAF/h1Kfqli9frosXL6p3796KiYmxT2vXrnVXfQAAAD7D6Vt1AAAADRW/VQcAAGASwQkAAMAkghMAAIBJBCcAAACTCE4AAAAmEZwAAABMIjgBAACYRHACAAAwieAEAABgEsEJAADAJIITAACASQQnAAAAkwhOAAAAJhGcAAAATCI4AQAAmERwAgAAMIngBAAAYBLBCQAAwCSCEwAAgEkEJwAAAJMITgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMAkghMAAIBJBCcAAACTCE4AAAAmEZwAAABMIjgBAACYRHACAAAwieAEAABgEsEJAADAJIITAACASQQnAAAAkwhOAAAAJhGcAAAATCI4AQAAmOR0cNq2bZsGDx6sZs2ayWKxaMOGDW4oCwAAwPc4HZwuXbqkhIQELVu2zB31AAAA+KwAZ98waNAgDRo0yB21AAAA+DSng5OzSkpKVFJSYp/Pz893ehs2m015eXmuLKtKmZmZbh/j1z799FO3j/nVV1+5dfuAr3FFz6mvPNVLJSk6OlpxcXEeGau+/o3Y86vz2N1jfFl8WSfKSj0yVl3n9uCUkpKi+fPn1/j9NptN8e3jVVxU7MKqvKv4fIlkkWbPnu3tUoB6p7Y9p77ydC8NDglWVmaW28NTff4b8c7lIr1zucj9A1qkhYUF7h/HCw4ePKiuXbu6dJtuD04zZszQtGnT7PP5+fmKjY01/f68vDwVFxUrcUqCwpqHuaNEu9yMXGWtPu7WMSSp7FKpZKhe7RPgK2rbc+orT/bSwjOF2rf0gPLy8twenPgbUTtX96k+Hb9fO3PmjMu36fbgZLVaZbVaa72dsOZhanxLhAsqurbCM4Vu3f6/q4/7BHibq3pOfeWJvuMN9bGfenKf6uPxcxe+xwkAAMAkp684FRYW6sSJE/b5U6dOaf/+/YqMjPTYg4AAAADe4HRw+uabb9SnTx/7/NVnCcaOHau0tDSXFQYAAOBrnA5OvXv3lmEY7qgFAADAp/GMEwAAgEkEJwAAAJMITgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMAkghMAAIBJBCcAAACTCE4AAAAmEZwAAABMIjgBAACYRHACAAAwieAEAABgEsEJAADAJIITAACASQQnAAAAkwhOAAAAJhGcAAAATCI4AQAAmERwAgAAMIngBAAAYBLBCQAAwCSCEwAAgEkEJwAAAJMITgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMAkghMAAIBJBCcAAACTCE4AAAAmEZwAAABMIjgBAACYRHACAAAwqUbB6fXXX1fLli0VHBysbt26afv27a6uCwAAwOc4HZzWrl2rKVOmaNasWdq3b5/uvvtuDRo0SDabzR31AQAA+Ayng9PLL7+s8ePH69FHH1X79u21dOlSxcbGavny5e6oDwAAwGc4FZyuXLmivXv36t5773VYfu+992rnzp0uLQwAAMDXBDizcl5ensrLy9WkSROH5U2aNFFOTk6V7ykpKVFJSYl9/uLFi5Kk/Px8U2MWFhb+8r6TF1VWXOZMuU4r+LHAI2N5ahzGYixvjyNJl368JEkqKioy/f97SQoPD5fFYnF6vNr2HEn64osv9M033zg9dk0EBASorMy9/xtIsvdoT/5v/vrrr6tp06ZuHcuT+1Uf//9ZX8e6eg4WFxe7vu8YTvjxxx8NScbOnTsdli9YsMCIj4+v8j1z5841JDExMTE5NV28eNGZ9kTPYWJiqvVkpu9YDMMwZNKVK1cUEhKi999/X//5n/9pX56cnKz9+/dr69atld7z7//6q6io0Llz5xQVFVWjf03WJ/n5+YqNjdUPP/yg6667ztvl+AyOS2UN8Zi46ooTPcdRQzyXfgvHpGoN8biY6TtO3aoLCgpSt27dtHnzZofgtHnzZg0dOrTK91itVlmtVodljRs3dmbYeu+6665rMCelMzgulXFMfhs9xxzOpco4JlXjuDhyKjhJ0rRp0zR69Gh1795dPXv21IoVK2Sz2fTHP/7RHfUBAAD4DKeD08MPP6yzZ8/queeeU3Z2tjp16qRPP/1ULVq0cEd9AAAAPsPp4CRJf/rTn/SnP/3J1bU0OFarVXPnzq10W6Gh47hUxjGBq3AuVcYxqRrHpWpOPRwOAADQkPEjvwAAACYRnAAAAEwiOAEAAJhEcHKxbdu2afDgwWrWrJksFos2bNjg8PoHH3ygAQMGKDo6WhaLRfv376+0jZKSEk2ePFnR0dEKDQ3VkCFDdObMGc/sgJu44rj07t1bFovFYRoxYoRndsANqjsmpaWleuaZZ9S5c2eFhoaqWbNmGjNmjP7v//7PYRv18VyB8+g7VaPvVEbfqT2Ck4tdunRJCQkJWrZs2TVfv/POO/XSSy9dcxtTpkzR+vXrtWbNGu3YsUOFhYV64IEHVF5e7q6y3c4Vx0WSJkyYoOzsbPv05ptvuqNcj6jumBQVFSkjI0OzZ89WRkaGPvjgAx07dkxDhgxxWK8+nitwHn2navSdyug7LlCjH4OCKZKM9evXV/naqVOnDEnGvn37HJZfuHDBCAwMNNasWWNf9uOPPxp+fn7Gpk2b3Fit59TkuBiGYSQlJRnJyclurc1bqjsmV3399deGJOP77783DKNhnCtwHn2navSdyug7NcMVJx+zd+9elZaW6t5777Uva9asmTp16qSdO3d6sTLf8N577yk6OlodO3bUU089pYKCAm+X5DEXL16UxWKx/3wI5wpchXOpevQd+s6v1egLMOE+OTk5CgoK0vXXX++wvEmTJsrJyfFSVb5h5MiRatmypZo2barDhw9rxowZOnDggDZv3uzt0tyuuLhYf/7zn/Xf//3f9t+M4lyBq3AuXRt9h77z7whOdYRhGA3+l90nTJhg/+9OnTqpTZs26t69uzIyMtS1a1cvVuZepaWlGjFihCoqKvT666//5vqcK3AVziX6Dn2nMm7V+ZimTZvqypUrOn/+vMPy3NxcNWnSxEtV+aauXbsqMDBQx48f93YpblNaWqrf/e53OnXqlDZv3uzwC+WcK3AVziXz6DucKwQnH9OtWzcFBgY6XAbOzs7W4cOH9R//8R9erMz3fPvttyotLVVMTIy3S3GLq83r+PHj2rJli6Kiohxe51yBq3AumUff4VzhVp2LFRYW6sSJE/b5U6dOaf/+/YqMjFRcXJzOnTsnm81m/16MrKwsSb+k+KZNmyoiIkLjx4/Xk08+qaioKEVGRuqpp55S586d1a9fP6/skyvU9ricPHlS7733nu677z5FR0fryJEjevLJJ5WYmKg777zTK/tUW9Udk2bNmumhhx5SRkaGNm7cqPLycvvzA5GRkQoKCqq35wqcR9+pGn2nMvqOC3j3Q331z5dffmlIqjSNHTvWMAzDSE1NrfL1uXPn2rdx+fJl4/HHHzciIyONRo0aGQ888IBhs9m8s0MuUtvjYrPZjF69ehmRkZFGUFCQccsttxhPPPGEcfbsWe/tVC1Vd0yufjy6qunLL7+0b6M+nitwHn2navSdyug7tWcxDMNwRQADAACo73jGCQAAwCSCEwAAgEkEJwAAAJMITgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMAkghNqbOfOnfL399fAgQMdlp8+fVoWi8U+hYeHq2PHjpo0aVKlXxRPS0tT48aNHZZdvnxZc+fOVXx8vKxWq6Kjo/XQQw/p22+/dVhv3rx5DuNcndq1a+eW/QXgXfQc+AKCE2rs7bff1uTJk7Vjxw7ZbLZKr2/ZskXZ2dk6cOCAXnzxRWVmZiohIUH//Oc/r7nNkpIS9evXT2+//baef/55HTt2TJ9++qnKy8vVo0cP7d6922H9jh07Kjs722HasWOHy/cVgPfRc+ALArxdAOqmS5cu6X//93+1Z88e5eTkKC0tTXPmzHFYJyoqSk2bNpUktWrVSoMHD1bfvn01fvx4nTx5Uv7+/pW2u3TpUu3atUv79u1TQkKCJKlFixZat26devToofHjx+vw4cOyWCySpICAAPsYAOoveg58BVecUCNr165VfHy84uPjNWrUKKWmpuq3fi/az89PycnJ+v7777V3794q11m9erX69+9vb2C/fu/UqVN15MgRHThwwGX7AaBuoOfAVxCcUCMrV67UqFGjJEkDBw5UYWFhtZfDr7r6LMDp06erfP3YsWNq3759la9dXX7s2DH7skOHDiksLMxhevTRR53ZFQB1AD0HvoJbdXBaVlaWvv76a33wwQeSfrl0/fDDD+vtt99Wv379qn3v1X8hXr3s7Yyq3hsfH6+PPvrIYb3w8HCntw3Ad9Fz4EsITnDaypUrVVZWpptuusm+zDAMBQYG6vz589W+NzMzU5LUsmXLKl9v27atjhw5UuVrR48elSS1adPGviwoKEitW7d2qn4AdQs9B76EW3VwSllZmd555x0tWbJE+/fvt08HDhxQixYt9N57713zvRUVFfrLX/6ili1bKjExscp1RowYoS1btlR6pqCiokKvvPKKOnToUOlZBAD1Fz0HvoYrTnDKxo0bdf78eY0fP14REREOrz300ENauXKlHnjgAUnS2bNnlZOTo6KiIh0+fFhLly7V119/rU8++aTKT7dI0tSpU/Xhhx9q8ODBWrJkiXr06KGffvrJ/tHiLVu2OFw2LysrU05OjsM2LBaLmjRp4uI9B+AN9Bz4GoITnLJy5Ur169evUgOTpGHDhunFF1/UuXPnJMn+7EFISIhatGihPn36aMWKFQ6XuSsqKhQQ8P9Pw+DgYH3xxRdKSUnRzJkz9f333ys8PFx9+vTR7t271alTJ4cxv/32W8XExDgss1qtKi4udtk+A/Aeeg58jcX4rc9zAm700ksv6d1339Xhw4e9XQqABoCeg9riihO8oqioSEePHlVqaqoGDRrk7XIA1HP0HLgKD4fDK1asWKF+/fopISGh0rf/AoCr0XPgKtyqAwAAMIkrTgAAACYRnAAAAEwiOAEAAJhEcAIAADCJ4AQAAGASwQkAAMAkghMAAIBJBCcAAACTCE4AAAAm/T9yqtympNo31wAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 600x300 with 2 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "bins = np.linspace(df1.ADJOE.min(), df1.ADJOE.max(), 10)\n",
    "g = sns.FacetGrid(df1, col=\"windex\", hue=\"POSTSEASON\", palette=\"Set1\", col_wrap=2)\n",
    "g.map(plt.hist, 'ADJOE', bins=bins, ec=\"k\")\n",
    "\n",
    "g.axes[-1].legend()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "# Pre-processing:  Feature selection/extraction\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Lets look at how Adjusted Defense Efficiency plots\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAk4AAAEiCAYAAAAPh11JAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMywgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/NK7nSAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAqLElEQVR4nO3deXQUZb7/8U8ngc7SIYFESIAkgkAA0RiCCy5sooAbXhlcLigoF/WIGMQLDiIiqMTryszVwYuXCajDwHUQRoOguARBXJAgi4aIiDYwycRgQhJC9uf3h0P/bAOxOkl3Z3m/zqlz7Oqqer79WHz5UF3dbTPGGAEAAOA3Bfi7AAAAgJaC4AQAAGARwQkAAMAighMAAIBFBCcAAACLCE4AAAAWEZwAAAAsIjgBAABYRHACAACwiODUBixfvlyRkZGNPs6wYcM0Y8aMRh+nuXr00Ud13nnn+bsMoNWhB6E1ITi1ATfddJO++eYbf5fhE8OGDZPNZquzVFdX+7s0oM1qCz3o+++/P2Xv+eXy6KOP+rtMNIEgfxcA7wsJCVFISIi/y/CZqVOnauHChW7rgoI41QF/aQs9KC4uTrm5ua7HzzzzjDZu3Kj33nvPtc7hcLj+2xijmpoaelMLxBWnFuitt95SZGSkamtrJUlffvmlbDabZs2a5drmrrvu0i233CKp7mXyk29JvfrqqzrzzDMVERGhm2++WSUlJa5tjh8/rttuu00Oh0OxsbF69tln69RRWVmp2bNnq1u3bgoLC9OFF16ozMxMSVJ5ebnOPvts3Xnnna7tDx48qIiICL388stNOR11hIaGKiYmxm2RpAcffFB9+vRRaGioevbsqXnz5qmqquq0x8nMzNQFF1ygsLAwRUZG6pJLLtEPP/zgev6tt95SSkqKgoOD1bNnTy1YsIArW2gT6EF1BQYGuvUch8OhoKAg1+N9+/YpPDxc77zzjgYNGiS73a4tW7Zo8uTJuv76692ONWPGDA0bNsz12Bijp556Sj179lRISIiSkpL0t7/9rclfA6whOLVAQ4YMUUlJiXbu3ClJ2rx5s6Kjo7V582bXNpmZmRo6dOhpj3HgwAGtW7dOGRkZysjI0ObNm/Xkk0+6np81a5Y+/PBDrV27Vu+++64yMzO1Y8cOt2Pcfvvt+vjjj7Vq1Srt3r1b48eP1+jRo7V//34FBwfrL3/5i1asWKF169appqZGt956q4YPH66pU6eetq4xY8bI4XDUuzRUeHi4li9frq+//lp/+MMf9PLLL+v5558/5bbV1dW6/vrrNXToUO3evVuffPKJ7rzzTtlsNknSO++8o4kTJ+q+++7T119/rf/5n//R8uXL9cQTTzS4PqCloAc13OzZs5WWlqbs7Gyde+65lvZ5+OGHlZ6eriVLluirr77S/fffr4kTJ7rNN3zIoEUaOHCgeeaZZ4wxxlx//fXmiSeeMO3btzfFxcUmNzfXSDLZ2dnGGGPS09NNRESEa9/58+eb0NBQU1xc7Fo3a9Ysc+GFFxpjjCkpKTHt27c3q1atcj1/9OhRExISYlJTU40xxnz77bfGZrOZI0eOuNV1+eWXmzlz5rgeP/XUUyY6OtpMnz7dxMTEmB9//LHe13X48GGzf//+epf6DB061LRr186EhYW5lpkzZ55y26eeesqkpKS4zUtSUpLr9UoymZmZp9z3sssuM4sWLXJb9+qrr5rY2Nh66wNaC3pQ/X7ZT4wx5sMPPzSSzLp169y2mzRpkhk7dqzbutTUVDN06FBjjDGlpaUmODjYbNu2zW2bKVOmmFtuucVSLWhavLnaQg0bNkyZmZmaOXOmtmzZoscff1xr1qzR1q1bVVRUpC5duqhv376n3f/MM89UeHi463FsbKzy8/Ml/fwvwcrKSg0ePNj1fKdOnZSYmOh6nJWVJWOM+vTp43bciooKRUVFuR4/8MAD+vvf/67//u//1oYNGxQdHV3v6+rWrZu1CajHhAkTNHfuXNfjk28R/O1vf9PixYv17bffqrS0VNXV1erQocMpj9GpUydNnjxZo0aN0hVXXKGRI0fqxhtvVGxsrCRpx44d2r59u9sVppqaGpWXl6usrEyhoaGNfh1Ac0YPaphBgwZ5tP3XX3+t8vJyXXHFFW7rKysrlZyc3JSlwSKCUws1bNgwLVu2TLt27VJAQID69++voUOHavPmzSosLKz3ErkktWvXzu2xzWZz3a9gjPnN8WtraxUYGKgdO3YoMDDQ7blfXsrOz89XTk6OAgMDtX//fo0ePbre444ZM0Zbtmypd5vS0tJ6n4+IiFCvXr3c1n366ae6+eabtWDBAo0aNUoRERFatWrVKe+bOCk9PV333XefNm7cqNWrV+vhhx/Wpk2bdNFFF6m2tlYLFizQDTfcUGe/4ODgeusDWgN6UMOEhYW5PQ4ICKjzen957+XJOVm/fn2dUGe32xtcBxqO4NRCnbzHYPHixRo6dKhsNpuGDh2qtLQ0FRYWKjU1tcHH7tWrl9q1a6dPP/1U8fHxkqTCwkJ98803rmaYnJysmpoa5efn67LLLjvtse644w4NGDBAU6dO1ZQpU3T55Zerf//+p93+f//3f3XixIkG1346H3/8sRISEtyuRP3yRu/TSU5OVnJysubMmaPBgwdr5cqVuuiiizRw4EDl5OTUCWhAW0EPahpnnHGG9u7d67buyy+/dAXL/v37y263y+l0/mYYhW8QnFqoiIgInXfeeXrttdf0hz/8QdLPjWz8+PGqqqpy+0SGpxwOh6ZMmaJZs2YpKipKXbp00dy5cxUQ8P8/S9CnTx9NmDBBt912m5599lklJyeroKBAH3zwgc455xxdddVVevHFF/XJJ59o9+7diouL04YNGzRhwgR99tlnat++/SnH9tZl8l69esnpdGrVqlU6//zztX79eq1du/a02x88eFBLly7Vddddp65duyonJ0fffPONbrvtNknSI488omuuuUZxcXEaP368AgICtHv3bu3Zs0ePP/64V14D0JzQg5rGiBEj9PTTT+uVV17R4MGD9dprr2nv3r2ut+HCw8P1n//5n7r//vtVW1urSy+9VMXFxdq2bZscDocmTZrk03rBp+patOHDh6umpsbVoDp27Kj+/fvrjDPOUL9+/Rp17KefflpDhgzRddddp5EjR+rSSy9VSkqK2zbp6em67bbb9MADDygxMVHXXXedPvvsM8XFxWnfvn2aNWuW/vSnPykuLk6S9OKLL6qoqEjz5s1rVG0NMXbsWN1///269957dd5552nbtm311hEaGqp9+/Zp3Lhx6tOnj+68807de++9uuuuuyRJo0aNUkZGhjZt2qTzzz9fF110kZ577jklJCT46iUBfkcParxRo0Zp3rx5mj17ts4//3yVlJS4/oF20mOPPaZHHnlEaWlp6tevn0aNGqW33npLPXr08FPVbZvNWHkzGQAAAFxxAgAAsIrgBAAAYBHBCQAAwCKCEwAAgEUEJwAAAIsITgAAABb5PDgZY1RcXGzpK/UBoCnQdwA0FZ8Hp5KSEkVERKikpMTXQwNoo+g7AJoKb9UBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARUH+LgAAADRcbW2tKisr/V1Gs9auXTsFBgY2ybEITgAAtFCVlZU6ePCgamtr/V1KsxcZGamYmBjZbLZGHYfgBABAC2SMUW5urgIDAxUXF6eAAO6+ORVjjMrKypSfny9Jio2NbdTxCE5tgNPpVEFBgU/Gio6OVnx8vE/GAoC2rLq6WmVlZeratatCQ0P9XU6zFhISIknKz89X586dG/W2HcGplXM6nUrsl6jysnKfjBccGqyc7BzCEwB4WU1NjSSpffv2fq6kZTgZLquqqghOOL2CggKVl5UreUaSHN0dXh2r9HCpdi7epYKCAoITAPhIY+/ZaSuaap4ITm2Eo7tDkWdF+LsMAABaNIITAACtiC/va5Xa3r2tBCcAAFoJp9OpfomJKiv3zX2tkhQaHKzsHOv3tk6ePFkrVqyos37UqFHauHGj8vLyNGvWLG3atEklJSVKTEzUQw89pN/97ndNXXqDEJwAAGglCgoKVFZerj9GdlLvIO//Fb+/ulr3Ff3k8b2to0ePVnp6uts6u90uSbr11lt17Ngxvfnmm4qOjtbKlSt100036YsvvlBycnKT1t8QBCcAAFqZ3kFBOqcZf9rObrcrJibmlM998sknWrJkiS644AJJ0sMPP6znn39eWVlZzSI48W1ZAACg2bj00ku1evVq/fTTT6qtrdWqVatUUVGhYcOG+bs0SQQnAADgYxkZGXI4HG7LY489JklavXq1qqurFRUVJbvdrrvuuktr167VWWed5eeqf8ZbdQAAwKeGDx+uJUuWuK3r1KmTpJ/fmissLNR7772n6OhorVu3TuPHj9eWLVt0zjnn+KNcNwQnAADgU2FhYerVq1ed9QcOHNALL7ygvXv36uyzz5YkJSUlacuWLXrxxRf10ksv+brUOnirDgAANAtlZWWSVOcHiwMDA1VbW+uPkurgihMAAPCpiooK5eXlua0LCgpS37591atXL91111165plnFBUVpXXr1mnTpk3KyMjwU7XuCE4AALQy+6urm/U4GzduVGxsrNu6xMRE7du3T2+//bZ+//vf69prr1Vpaal69eqlFStW6KqrrmqKkhuN4AQAQCsRHR2t0OBg3Vf0k8/GDA0OVnR0tOXtly9fruXLl5/2+d69e2vNmjVNUJl3EJwAAGgl4uPjlZ2Tw2/VeRHBCQCAViQ+Pr5NBRlf41N1AAAAFhGcAAAALCI4AQAAWERwAgAAsIjgBAAAYBHBCQAAwCKCEwAAgEUef4/TkSNH9OCDD2rDhg06ceKE+vTpo2XLliklJcUb9QEAAA84nU6+ANOLPApOhYWFuuSSSzR8+HBt2LBBnTt31oEDBxQZGeml8gAAgFVOp1OJffup/ESZz8YMDglVzr5sy+Fp8uTJWrFiRZ31+/fvV69evVyP09LS9NBDDyk1NVWLFy9uqnIbzaPg9F//9V+Ki4tTenq6a92ZZ57Z1DUBAIAGKCgoUPmJMp31b7MVEh3n9fFOFBzSgbVPqaCgwKOrTqNHj3bLEpJ0xhlnuP57+/btWrp0qc4999wmq7WpeBSc3nzzTY0aNUrjx4/X5s2b1a1bN91zzz2aOnXqafepqKhQRUWF63FxcXHDqwUAC+g7aOtCouMUFtvb32Wclt1uV0xMzCmfKy0t1YQJE/Tyyy/r8ccf93Flv82jm8O/++47LVmyRL1799Y777yju+++W/fdd59eeeWV0+6TlpamiIgI1xIX5/0EDKBto+8ALde0adN09dVXa+TIkf4u5ZQ8Ck61tbUaOHCgFi1apOTkZN11112aOnWqlixZctp95syZo2PHjrmWQ4cONbpoAKgPfQdo3jIyMuRwOFzL+PHjJUmrVq1SVlaW0tLS/Fzh6Xn0Vl1sbKz69+/vtq5fv35as2bNafex2+2y2+0Nqw4AGoC+AzRvw4cPd7voEhYWpkOHDik1NVXvvvuugoOD/Vhd/TwKTpdccolycnLc1n3zzTdKSEho0qIAAEDrFRYW5vYJOklat26d8vPz3b7eqKamRh999JFeeOEFVVRUKDAw0Nel1uFRcLr//vt18cUXa9GiRbrxxhv1+eefa+nSpVq6dKm36gMAAG3A5Zdfrj179ritu/3229W3b189+OCDzSI0SR4Gp/PPP19r167VnDlztHDhQvXo0UOLFy/WhAkTvFUfAABoA8LDwzVgwAC3dWFhYYqKiqqz3p88/ubwa665Rtdcc403agEAAE3gRIFvPhDhq3GaE4+DEwAAaJ6io6MVHBKqA2uf8tmYwSGhio6Otrz98uXLLW+bmZnpeUFeRnACAKCViI+PV86+bH6rzosITgAAtCLx8fFtKsj4mkdfgAkAANCWEZwAAAAsIjgBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARXyPEwAArYjT6eQLML2I4AQAQCvhdDqV2C9R5WXlPhszODRYOdk5HoWn/Px8zZs3Txs2bNA///lPdezYUUlJSXr00Uc1ePBgLV26VCtXrlRWVpZKSkpUWFioyMjIOsdZv369Fi5cqN27dyssLExDhgzRG2+80YSvri6CEwAArURBQYHKy8qVPCNJju4Or49XerhUOxfvUkFBgUfBady4caqqqtKKFSvUs2dP/fOf/9T777+vn376SZJUVlam0aNHa/To0ZozZ84pj7FmzRpNnTpVixYt0ogRI2SM0Z49e5rkddWH4AQAQCvj6O5Q5FkR/i7jlIqKirR161ZlZmZq6NChkqSEhARdcMEFrm1mzJgh6fQ/8ltdXa3U1FQ9/fTTmjJlimt9YmKi1+o+iZvDAQCAzzgcDjkcDq1bt04VFRUNOkZWVpaOHDmigIAAJScnKzY2VmPGjNFXX33VxNXWRXACAAA+ExQUpOXLl2vFihWKjIzUJZdcooceeki7d++2fIzvvvtOkvToo4/q4YcfVkZGhjp27KihQ4e63u7zFoITAADwqXHjxukf//iH3nzzTY0aNUqZmZkaOHCgli9fbmn/2tpaSdLcuXM1btw4paSkKD09XTabTa+//roXKyc4AQAAPwgODtYVV1yhRx55RNu2bdPkyZM1f/58S/vGxsZKkvr37+9aZ7fb1bNnTzmdTq/UexLBCQAA+F3//v11/PhxS9umpKTIbrcrJyfHta6qqkrff/+9EhISvFWiJD5VBwAAfOjo0aMaP3687rjjDp177rkKDw/XF198oaeeekpjx46VJOXl5SkvL0/ffvutJGnPnj0KDw9XfHy8OnXqpA4dOujuu+/W/PnzFRcXp4SEBD399NOSpPHjx3u1foITAACtTOnh0mY7jsPh0IUXXqjnn39eBw4cUFVVleLi4jR16lQ99NBDkqSXXnpJCxYscO0zZMgQSVJ6eromT54sSXr66acVFBSkW2+9VSdOnNCFF16oDz74QB07dmz8C6uHzRhjvDrCrxQXFysiIkLHjh1Thw4dfDl0m5SVlaWUlBRd9swlXv9Oj6IDx7TlPz/Wjh07NHDgQK+OJfn2ZwXa2k8KtDb0HbRG5eXlOnjwoHr06KHg4GBJLeebw/3hVPPVEFxxQovk6+bQUhoDgLYtPj5eOdk5/FadFxGc0CL58mcFGvqTAgDgD/Hx8fQqLyI4oUVrzj8rAABoffg6AgAAAIsITgAAABYRnAAAaMF8/OH4Fuvkz7Q0Fvc4AQDQArVr1042m00//vijzjjjDNlsNn+X1CwZY1RZWakff/xRAQEBat++faOOR3ACAKAFCgwMVPfu3XX48GF9//33/i6n2QsNDVV8fLwCAhr3ZhvBCQCAFsrhcKh3796qqqrydynNWmBgoIKCgprkqhzBCQCAFiwwMFCBgYH+LqPN4OZwAAAAiwhOAAAAFhGcAAAALCI4AQAAWERwAgAAsIjgBAAAYBHBCQAAwCKCEwAAgEUEJwAAAIsITgAAABYRnAAAACwiOAEAAFhEcAIAALCoUcEpLS1NNptNM2bMaKJyAAAAmq8GB6ft27dr6dKlOvfcc5uyHgAAgGarQcGptLRUEyZM0Msvv6yOHTs2dU0AAADNUoOC07Rp03T11Vdr5MiRTV0PAABAsxXk6Q6rVq1SVlaWtm/fbmn7iooKVVRUuB4XFxd7OiRamLffflvZ2dleHePgwYNePf6pePs1SVJ0dLTi4+O9Pk5rR985NafTqYKCAp+MxbmM1sqj4HTo0CGlpqbq3XffVXBwsKV90tLStGDBggYVh5alvLBCsknz5s3zdylN6uTrmjhxotfHCg4NVk52Dn/hNBJ9py6n06l+iYkqKy/3yXihwcHKzuFcRuvjUXDasWOH8vPzlZKS4lpXU1Ojjz76SC+88IIqKioUGBjots+cOXM0c+ZM1+Pi4mLFxcU1smw0R9XHqyQjJc9IkqO7w6tj5WflK2flfq+OcZKvXlfp4VLtXLxLBQUF/GXTSPSdugoKClRWXq4/RnZS7yCP32zwyP7qat1X9BPnMlolj/70XH755dqzZ4/buttvv119+/bVgw8+WCc0SZLdbpfdbm9clWhRHN0dijwrwqtjlB4u9erxT8UXrwtNg75zer2DgnRO+/b+LgNosTwKTuHh4RowYIDburCwMEVFRdVZDwAA0NrwzeEAAAAWNfqN7szMzCYoAwAAoPnjihMAAIBFBCcAAACLCE4AAAAWEZwAAAAsIjgBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARQQnAAAAiwhOAAAAFhGcAAAALCI4AQAAWERwAgAAsIjgBAAAYBHBCQAAwCKCEwAAgEUEJwAAAIsITgAAABYRnAAAACwiOAEAAFgU5O8CAAC+8211VasYw1+cTqcKCgq8Pk50dLTi4+O9Pg48R3ACgDYgNzdXsknTiwp9M6DtX2O2Ik6nU4n9ElVeVu71sYJDg5WTnUN4aoYITgDQBhQVFUlGSp6RJEd3h1fHKj1cqp2Ld/08ZitSUFCg8rJyr8/hyfkrKCggODVDBCcAaEMc3R2KPCvC32W0aMxh28bN4QAAABYRnAAAACwiOAEAAFhEcAIAALCI4AQAAGARwQkAAMAighMAAIBFBCcAAACLCE4AAAAWEZwAAAAsIjgBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARQQnAAAAiwhOAAAAFhGcAAAALCI4AQAAWERwAgAAsIjgBAAAYJFHwSktLU3nn3++wsPD1blzZ11//fXKycnxVm0AAADNikfBafPmzZo2bZo+/fRTbdq0SdXV1bryyit1/Phxb9UHAADQbAR5svHGjRvdHqenp6tz587asWOHhgwZ0qSFAQAANDceBadfO3bsmCSpU6dOp92moqJCFRUVrsfFxcWNGRIAfhN9p21xOp0qKCjw+jjZ2dleHwPNX4ODkzFGM2fO1KWXXqoBAwacdru0tDQtWLCgocOgkXJzc/1dAuBz9J22w+l0KrFfosrLyv1dCtqIBgene++9V7t379bWrVvr3W7OnDmaOXOm63FxcbHi4uIaOiw8VFRU5O8SAJ+j77QdBQUFKi8rV/KMJDm6O7w6Vn5WvnJW7vfqGGj+GhScpk+frjfffFMfffSRunfvXu+2drtddru9QcUBQEPQd9oeR3eHIs+K8OoYpYdLvXp8tAweBSdjjKZPn661a9cqMzNTPXr08FZdAAAAzY5HwWnatGlauXKl/v73vys8PFx5eXmSpIiICIWEhHilQAAAgObCo+9xWrJkiY4dO6Zhw4YpNjbWtaxevdpb9QEAADQbHr9VBwAA0FbxW3UAAAAWEZwAAAAsIjgBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARQQnAAAAiwhOAAAAFhGcAAAALCI4AQAAWERwAgAAsIjgBAAAYBHBCQAAwCKCEwAAgEUEJwAAAIsITgAAABYRnAAAACwiOAEAAFhEcAIAALCI4AQAAGBRkL8LaE7Wrl2rr776yidjde7cWYMGDfL6OAcPHvT6GGha2dnZPhnnhx9+UFlZmU/G6tmzpwYPHuyTsZqK0+lUQUGB18epqKiQ3W73+jj0AqBpEJz+Ze3atbph3A2S8dGANvluLLQI5YUVkk2aOHGibwb04TloC7Dp460ft5jw5HQ6ldgvUeVl5V4fyxZgk6ltnc3AF8Fz9+7dXh/DX3Jzc/1dAk6B4PQvX331lWSk5BlJcnR3eHWs0sOl2rl4l24LCdMtYWFeHeuvx4/rlRPHvToGmkb18SqfnYP5WfnKWbnfp+f7d99912KCU0FBgcrLyr0+PyfnZpajg0YEB3ttHEl6oaRY6yu8HwR/qaSkxOtjHD582Otj+EtRUZG/S8ApEJx+xdHdocizInwyVkxggM5p396rY3xYfsKrx0fT88U5WHq41GdjtWS+mp/4oECv94KoAG5pBZoCf5IAAAAsIjgBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARQQnAAAAiwhOAAAAFhGcAAAALCI4AQAAWERwAgAAsIjgBAAAYBHBCQAAwCKCEwAAgEUEJwAAAIsITgAAABYRnAAAACwiOAEAAFhEcAIAALCI4AQAAGBRg4LTn/70J/Xo0UPBwcFKSUnRli1bmrouAACAZsfj4LR69WrNmDFDc+fO1c6dO3XZZZdpzJgxcjqd3qgPAACg2fA4OD333HOaMmWK/uM//kP9+vXT4sWLFRcXpyVLlnijPgAAgGbDo+BUWVmpHTt26Morr3Rbf+WVV2rbtm1NWhgAAEBzE+TJxgUFBaqpqVGXLl3c1nfp0kV5eXmn3KeiokIVFRWux8eOHZMkFRcXWx73gw8+0BdffOFJqR7bunWrJOnYgWOqLq/26ljHjxyXJH1UXq4KY7w61mf/mntfvK6SIyWM1QLG8fVYJ8/3srIyj/7ch4eHy2azNWjMxvad0tLSn/fz8vycnJu3ysr0bVWV18aRpJ2VvusFJ1/Xe++9p/Lycq+O5cve7as/Nyfn7/XXX1d2drbXxjkpKChI1dXenTt/jDVo0CCNGDHCo30s9R3jgSNHjhhJZtu2bW7rH3/8cZOYmHjKfebPn28ksbCwsHi0HDt2zJP2RN9hYWFp9GKl79iMsX7Jo7KyUqGhoXr99df1b//2b671qamp+vLLL7V58+Y6+/z6X361tbX66aefFBUVpZKSEsXFxenQoUPq0KGD1TJateLiYubkF5gPd21pPpryihN9p35t6byygvlw15bmw0rf8eituvbt2yslJUWbNm1yC06bNm3S2LFjT7mP3W6X3W53WxcZGSlJruI6dOjQ6v9neIo5ccd8uGM+6kffaRjmxB3z4Y75+JlHwUmSZs6cqVtvvVWDBg3S4MGDtXTpUjmdTt19993eqA8AAKDZ8Dg43XTTTTp69KgWLlyo3NxcDRgwQG+//bYSEhK8UR8AAECz4XFwkqR77rlH99xzT6MHt9vtmj9/fp1L6m0Zc+KO+XDHfDQec1gXc+KO+XDHfLjz6OZwAACAtowf+QUAALCI4AQAAGARwQkAAMAinwSn6upqPfzww+rRo4dCQkLUs2dPLVy4ULW1ta5tJk+eLJvN5rZcdNFFvijPL0pKSjRjxgwlJCQoJCREF198sbZv3+563hijRx99VF27dlVISIiGDRumr776yo8Ve9dvzUdrPj8++ugjXXvtteratatsNpvWrVvn9ryVc6GiokLTp09XdHS0wsLCdN111+nw4cM+fBXND32nLvqOO/oOfadBGvybBh54/PHHTVRUlMnIyDAHDx40r7/+unE4HGbx4sWubSZNmmRGjx5tcnNzXcvRo0d9UZ5f3HjjjaZ///5m8+bNZv/+/Wb+/PmmQ4cO5vDhw8YYY5588kkTHh5u1qxZY/bs2WNuuukmExsba4qLi/1cuXf81ny05vPj7bffNnPnzjVr1qwxkszatWvdnrdyLtx9992mW7duZtOmTSYrK8sMHz7cJCUlmerqah+/muaDvlMXfccdfYe+0xA+CU5XX321ueOOO9zW3XDDDWbixImux5MmTTJjx471RTl+V1ZWZgIDA01GRobb+qSkJDN37lxTW1trYmJizJNPPul6rry83ERERJiXXnrJ1+V63W/NhzFt5/z4dQOzci4UFRWZdu3amVWrVrm2OXLkiAkICDAbN270We3NDX3HHX3HHX3n/6PveMYnb9Vdeumlev/99/XNN99Iknbt2qWtW7fqqquuctsuMzNTnTt3Vp8+fTR16lTl5+f7ojyfq66uVk1NjYKDg93Wh4SEaOvWrTp48KDy8vJ05ZVXup6z2+0aOnSotm3b5utyve635uOktnJ+/JKVc2HHjh2qqqpy26Zr164aMGBAqzxfrKLvuKPvuKPvnB59p34N+gJMTz344IM6duyY+vbtq8DAQNXU1OiJJ57QLbfc4tpmzJgxGj9+vBISEnTw4EHNmzdPI0aM0I4dO1rdl26Fh4dr8ODBeuyxx9SvXz916dJFf/3rX/XZZ5+pd+/eysvLkyR16dLFbb8uXbrohx9+8EfJXvVb8yG1rfPjl6ycC3l5eWrfvr06duxYZ5uT+7dF9B139B139J3To+/UzyfBafXq1Xrttde0cuVKnX322fryyy81Y8YMde3aVZMmTZL080+5nDRgwAANGjRICQkJWr9+vW644QZflOlTr776qu644w5169ZNgYGBGjhwoP793/9dWVlZrm1+/QvNxpgG/1p8c/db89HWzo9fa8i50JrPFyvoO3XRd9zRd+pH3zk1n7xVN2vWLP3+97/XzTffrHPOOUe33nqr7r//fqWlpZ12n9jYWCUkJGj//v2+KNHnzjrrLG3evFmlpaU6dOiQPv/8c1VVValHjx6KiYmRpDqpPT8/v86/AFqL+ubjVFr7+XGSlXMhJiZGlZWVKiwsPO02bRF9py76jjv6zqnRd+rnk+BUVlamgAD3oQIDA90+FvxrR48e1aFDhxQbG+vt8vwqLCxMsbGxKiws1DvvvKOxY8e6mtimTZtc21VWVmrz5s26+OKL/Vit951qPk6lrZwfVs6FlJQUtWvXzm2b3Nxc7d27t9WfL/Wh75wefccdfccdfec3+OIO9EmTJplu3bq5Phb8xhtvmOjoaDN79mxjjDElJSXmgQceMNu2bTMHDx40H374oRk8eLDp1q1bq/0Y7MaNG82GDRvMd999Z959912TlJRkLrjgAlNZWWmM+fmjoBEREeaNN94we/bsMbfcckur/lhwffPR2s+PkpISs3PnTrNz504jyTz33HNm586d5ocffjDGWDsX7r77btO9e3fz3nvvmaysLDNixIg28bHg+tB36qLvuKPv0HcawifBqbi42KSmppr4+HgTHBxsevbsaebOnWsqKiqMMT9/LPTKK680Z5xxhmnXrp2Jj483kyZNMk6n0xfl+cXq1atNz549Tfv27U1MTIyZNm2aKSoqcj1fW1tr5s+fb2JiYozdbjdDhgwxe/bs8WPF3lXffLT28+PDDz80kuoskyZNMsZYOxdOnDhh7r33XtOpUycTEhJirrnmmlYzPw1F36mLvuOOvkPfaQibMcb451oXAABAy8Jv1QEAAFhEcAIAALCI4AQAAGARwQkAAMAighMAAIBFBCcAAACLCE4AAAAWEZwAAAAsIjgBAABYRHBCg23btk2BgYEaPXq02/rvv/9eNpvNtYSHh+vss8/WtGnT6vyq+PLlyxUZGen2+OR+gYGB6tixoy688EItXLhQx44dc9t38uTJbuOcXH5dD4DWgZ6D5oDghAb785//rOnTp2vr1q1yOp11nn/vvfeUm5urXbt2adGiRcrOzlZSUpLef//9eo/boUMH5ebm6vDhw9q2bZvuvPNOvfLKKzrvvPP0j3/8w23b0aNHKzc3123561//2qSvE0DzQM9BcxDk7wLQMh0/flz/93//p+3btysvL0/Lly/XI4884rZNVFSUYmJiJEk9e/bUtddeq8svv1xTpkzRgQMHFBgYeMpj22w2136xsbHq16+frr32Wp199tmaPXu2XnvtNde2drvdtS2A1oueg+aCK05okNWrVysxMVGJiYmaOHGi0tPT9Vu/Fx0QEKDU1FT98MMP2rFjh0fjde7cWRMmTNCbb76pmpqaxpQOoAWi56C5IDihQZYtW6aJEydK+vnSdWlp6W9eDpekvn37Svr5ngRP9e3bVyUlJTp69KhrXUZGhhwOh9vy2GOPeXxsAM0bPQfNBW/VwWM5OTn6/PPP9cYbb0iSgoKCdNNNN+nPf/6zRo4cWe++J/+FaLPZPB73VPsOHz5cS5YscduuU6dOHh8bQPNFz0FzQnCCx5YtW6bq6mp169bNtc4Yo3bt2qmwsLDefbOzsyVJPXr08Hjc7OxsdejQQVFRUa51YWFh6tWrl8fHAtBy0HPQnPBWHTxSXV2tV155Rc8++6y+/PJL17Jr1y4lJCToL3/5y2n3ra2t1R//+Ef16NFDycnJHo2bn5+vlStX6vrrr1dAAKct0FbQc9DccMUJHsnIyFBhYaGmTJmiiIgIt+d+97vfadmyZbrmmmskSUePHlVeXp7Kysq0d+9eLV68WJ9//rnWr19/2k+3SD//SzIvL0/GGBUVFemTTz7RokWLFBERoSeffNJt24qKCuXl5bmtCwoKUnR0dBO9YgD+RM9Bc0NwgkeWLVumkSNH1mlgkjRu3DgtWrRIP/30kyS57j0IDQ1VQkKChg8frqVLl7pd5q6trVVQkPtpWFxcrNjYWNlsNnXo0EGJiYmaNGmSUlNT1aFDB7dtN27cqNjYWLd1iYmJ2rdvX5O8XgD+Rc9Bc2Mzv/V5TsCLnnzySb322mvau3evv0sB0AbQc9BYXHGCX5SVlWnfvn1KT0/XmDFj/F0OgFaOnoOmwh1v8IulS5dq5MiRSkpKqvPtvwDQ1Og5aCq8VQcAAGARV5wAAAAsIjgBAABYRHACAACwiOAEAABgEcEJAADAIoITAACARQQnAAAAiwhOAAAAFhGcAAAALPp/XQxkhktZrroAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 600x300 with 2 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "bins = np.linspace(df1.ADJDE.min(), df1.ADJDE.max(), 10)\n",
    "g = sns.FacetGrid(df1, col=\"windex\", hue=\"POSTSEASON\", palette=\"Set1\", col_wrap=2)\n",
    "g.map(plt.hist, 'ADJDE', bins=bins, ec=\"k\")\n",
    "g.axes[-1].legend()\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "We see that this data point doesn't impact the ability of a team to get into the Final Four. \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "## Convert Categorical features to numerical values\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Lets look at the postseason:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "windex  POSTSEASON\n",
       "False   S16           0.605263\n",
       "        E8            0.263158\n",
       "        F4            0.131579\n",
       "True    S16           0.500000\n",
       "        E8            0.333333\n",
       "        F4            0.166667\n",
       "Name: POSTSEASON, dtype: float64"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df1.groupby(['windex'])['POSTSEASON'].value_counts(normalize=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "13% of teams with 6 or less wins above bubble make it into the final four while 17% of teams with 7 or more do.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Lets convert wins above bubble (winindex) under 7 to 0 and over 7 to 1:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/pandas/core/generic.py:6619: SettingWithCopyWarning: \n",
      "A value is trying to be set on a copy of a slice from a DataFrame\n",
      "\n",
      "See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy\n",
      "  return self._update_inplace(result)\n"
     ]
    },
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
       "      <th>TEAM</th>\n",
       "      <th>CONF</th>\n",
       "      <th>G</th>\n",
       "      <th>W</th>\n",
       "      <th>ADJOE</th>\n",
       "      <th>ADJDE</th>\n",
       "      <th>BARTHAG</th>\n",
       "      <th>EFG_O</th>\n",
       "      <th>EFG_D</th>\n",
       "      <th>TOR</th>\n",
       "      <th>...</th>\n",
       "      <th>2P_O</th>\n",
       "      <th>2P_D</th>\n",
       "      <th>3P_O</th>\n",
       "      <th>3P_D</th>\n",
       "      <th>ADJ_T</th>\n",
       "      <th>WAB</th>\n",
       "      <th>POSTSEASON</th>\n",
       "      <th>SEED</th>\n",
       "      <th>YEAR</th>\n",
       "      <th>windex</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Notre Dame</td>\n",
       "      <td>ACC</td>\n",
       "      <td>36</td>\n",
       "      <td>24</td>\n",
       "      <td>118.3</td>\n",
       "      <td>103.3</td>\n",
       "      <td>0.8269</td>\n",
       "      <td>54.0</td>\n",
       "      <td>49.5</td>\n",
       "      <td>15.3</td>\n",
       "      <td>...</td>\n",
       "      <td>52.9</td>\n",
       "      <td>46.5</td>\n",
       "      <td>37.4</td>\n",
       "      <td>36.9</td>\n",
       "      <td>65.5</td>\n",
       "      <td>2.3</td>\n",
       "      <td>E8</td>\n",
       "      <td>6.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Virginia</td>\n",
       "      <td>ACC</td>\n",
       "      <td>37</td>\n",
       "      <td>29</td>\n",
       "      <td>119.9</td>\n",
       "      <td>91.0</td>\n",
       "      <td>0.9600</td>\n",
       "      <td>54.8</td>\n",
       "      <td>48.4</td>\n",
       "      <td>15.1</td>\n",
       "      <td>...</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.3</td>\n",
       "      <td>40.3</td>\n",
       "      <td>34.7</td>\n",
       "      <td>61.9</td>\n",
       "      <td>8.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Kansas</td>\n",
       "      <td>B12</td>\n",
       "      <td>37</td>\n",
       "      <td>32</td>\n",
       "      <td>120.9</td>\n",
       "      <td>90.4</td>\n",
       "      <td>0.9662</td>\n",
       "      <td>55.7</td>\n",
       "      <td>45.1</td>\n",
       "      <td>17.8</td>\n",
       "      <td>...</td>\n",
       "      <td>52.7</td>\n",
       "      <td>43.4</td>\n",
       "      <td>41.3</td>\n",
       "      <td>32.5</td>\n",
       "      <td>70.1</td>\n",
       "      <td>11.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Oregon</td>\n",
       "      <td>P12</td>\n",
       "      <td>37</td>\n",
       "      <td>30</td>\n",
       "      <td>118.4</td>\n",
       "      <td>96.2</td>\n",
       "      <td>0.9163</td>\n",
       "      <td>52.3</td>\n",
       "      <td>48.9</td>\n",
       "      <td>16.1</td>\n",
       "      <td>...</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.1</td>\n",
       "      <td>34.4</td>\n",
       "      <td>36.2</td>\n",
       "      <td>69.0</td>\n",
       "      <td>6.7</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Syracuse</td>\n",
       "      <td>ACC</td>\n",
       "      <td>37</td>\n",
       "      <td>23</td>\n",
       "      <td>111.9</td>\n",
       "      <td>93.6</td>\n",
       "      <td>0.8857</td>\n",
       "      <td>50.0</td>\n",
       "      <td>47.3</td>\n",
       "      <td>18.1</td>\n",
       "      <td>...</td>\n",
       "      <td>47.2</td>\n",
       "      <td>48.1</td>\n",
       "      <td>36.0</td>\n",
       "      <td>30.7</td>\n",
       "      <td>65.5</td>\n",
       "      <td>-0.3</td>\n",
       "      <td>F4</td>\n",
       "      <td>10.0</td>\n",
       "      <td>2016</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 25 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "         TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  ...  \\\n",
       "2  Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3  ...   \n",
       "3    Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1  ...   \n",
       "4      Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8  ...   \n",
       "5      Oregon  P12  37  30  118.4   96.2   0.9163   52.3   48.9  16.1  ...   \n",
       "6    Syracuse  ACC  37  23  111.9   93.6   0.8857   50.0   47.3  18.1  ...   \n",
       "\n",
       "   2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  windex  \n",
       "2  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016       0  \n",
       "3  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016       1  \n",
       "4  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016       1  \n",
       "5  52.6  46.1  34.4  36.2   69.0   6.7          E8   1.0  2016       0  \n",
       "6  47.2  48.1  36.0  30.7   65.5  -0.3          F4  10.0  2016       0  \n",
       "\n",
       "[5 rows x 25 columns]"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df1['windex'].replace(to_replace=['False','True'], value=[0,1],inplace=True)\n",
    "df1.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Feature selection\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let's define feature sets, X:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
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
       "      <th>G</th>\n",
       "      <th>W</th>\n",
       "      <th>ADJOE</th>\n",
       "      <th>ADJDE</th>\n",
       "      <th>BARTHAG</th>\n",
       "      <th>EFG_O</th>\n",
       "      <th>EFG_D</th>\n",
       "      <th>TOR</th>\n",
       "      <th>TORD</th>\n",
       "      <th>ORB</th>\n",
       "      <th>...</th>\n",
       "      <th>FTR</th>\n",
       "      <th>FTRD</th>\n",
       "      <th>2P_O</th>\n",
       "      <th>2P_D</th>\n",
       "      <th>3P_O</th>\n",
       "      <th>3P_D</th>\n",
       "      <th>ADJ_T</th>\n",
       "      <th>WAB</th>\n",
       "      <th>SEED</th>\n",
       "      <th>windex</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>36</td>\n",
       "      <td>24</td>\n",
       "      <td>118.3</td>\n",
       "      <td>103.3</td>\n",
       "      <td>0.8269</td>\n",
       "      <td>54.0</td>\n",
       "      <td>49.5</td>\n",
       "      <td>15.3</td>\n",
       "      <td>14.8</td>\n",
       "      <td>32.7</td>\n",
       "      <td>...</td>\n",
       "      <td>32.9</td>\n",
       "      <td>26.0</td>\n",
       "      <td>52.9</td>\n",
       "      <td>46.5</td>\n",
       "      <td>37.4</td>\n",
       "      <td>36.9</td>\n",
       "      <td>65.5</td>\n",
       "      <td>2.3</td>\n",
       "      <td>6.0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>37</td>\n",
       "      <td>29</td>\n",
       "      <td>119.9</td>\n",
       "      <td>91.0</td>\n",
       "      <td>0.9600</td>\n",
       "      <td>54.8</td>\n",
       "      <td>48.4</td>\n",
       "      <td>15.1</td>\n",
       "      <td>18.8</td>\n",
       "      <td>29.9</td>\n",
       "      <td>...</td>\n",
       "      <td>32.1</td>\n",
       "      <td>33.4</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.3</td>\n",
       "      <td>40.3</td>\n",
       "      <td>34.7</td>\n",
       "      <td>61.9</td>\n",
       "      <td>8.6</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>37</td>\n",
       "      <td>32</td>\n",
       "      <td>120.9</td>\n",
       "      <td>90.4</td>\n",
       "      <td>0.9662</td>\n",
       "      <td>55.7</td>\n",
       "      <td>45.1</td>\n",
       "      <td>17.8</td>\n",
       "      <td>18.5</td>\n",
       "      <td>32.2</td>\n",
       "      <td>...</td>\n",
       "      <td>38.6</td>\n",
       "      <td>37.3</td>\n",
       "      <td>52.7</td>\n",
       "      <td>43.4</td>\n",
       "      <td>41.3</td>\n",
       "      <td>32.5</td>\n",
       "      <td>70.1</td>\n",
       "      <td>11.6</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>37</td>\n",
       "      <td>30</td>\n",
       "      <td>118.4</td>\n",
       "      <td>96.2</td>\n",
       "      <td>0.9163</td>\n",
       "      <td>52.3</td>\n",
       "      <td>48.9</td>\n",
       "      <td>16.1</td>\n",
       "      <td>20.2</td>\n",
       "      <td>34.1</td>\n",
       "      <td>...</td>\n",
       "      <td>40.3</td>\n",
       "      <td>32.0</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.1</td>\n",
       "      <td>34.4</td>\n",
       "      <td>36.2</td>\n",
       "      <td>69.0</td>\n",
       "      <td>6.7</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>37</td>\n",
       "      <td>23</td>\n",
       "      <td>111.9</td>\n",
       "      <td>93.6</td>\n",
       "      <td>0.8857</td>\n",
       "      <td>50.0</td>\n",
       "      <td>47.3</td>\n",
       "      <td>18.1</td>\n",
       "      <td>20.4</td>\n",
       "      <td>33.5</td>\n",
       "      <td>...</td>\n",
       "      <td>35.4</td>\n",
       "      <td>28.0</td>\n",
       "      <td>47.2</td>\n",
       "      <td>48.1</td>\n",
       "      <td>36.0</td>\n",
       "      <td>30.7</td>\n",
       "      <td>65.5</td>\n",
       "      <td>-0.3</td>\n",
       "      <td>10.0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 21 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "    G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  TORD   ORB  ...   FTR  \\\n",
       "2  36  24  118.3  103.3   0.8269   54.0   49.5  15.3  14.8  32.7  ...  32.9   \n",
       "3  37  29  119.9   91.0   0.9600   54.8   48.4  15.1  18.8  29.9  ...  32.1   \n",
       "4  37  32  120.9   90.4   0.9662   55.7   45.1  17.8  18.5  32.2  ...  38.6   \n",
       "5  37  30  118.4   96.2   0.9163   52.3   48.9  16.1  20.2  34.1  ...  40.3   \n",
       "6  37  23  111.9   93.6   0.8857   50.0   47.3  18.1  20.4  33.5  ...  35.4   \n",
       "\n",
       "   FTRD  2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  SEED  windex  \n",
       "2  26.0  52.9  46.5  37.4  36.9   65.5   2.3   6.0       0  \n",
       "3  33.4  52.6  46.3  40.3  34.7   61.9   8.6   1.0       1  \n",
       "4  37.3  52.7  43.4  41.3  32.5   70.1  11.6   1.0       1  \n",
       "5  32.0  52.6  46.1  34.4  36.2   69.0   6.7   1.0       0  \n",
       "6  28.0  47.2  48.1  36.0  30.7   65.5  -0.3  10.0       0  \n",
       "\n",
       "[5 rows x 21 columns]"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X = df1[['G', 'W', 'ADJOE', 'ADJDE', 'BARTHAG', 'EFG_O', 'EFG_D',\n",
    "       'TOR', 'TORD', 'ORB', 'DRB', 'FTR', 'FTRD', '2P_O', '2P_D', '3P_O',\n",
    "       '3P_D', 'ADJ_T', 'WAB', 'SEED', 'windex']]\n",
    "X[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "What are our lables? Round where the given team was eliminated or where their season ended (R68 = First Four, R64 = Round of 64, R32 = Round of 32, S16 = Sweet Sixteen, E8 = Elite Eight, F4 = Final Four, 2ND = Runner-up, Champion = Winner of the NCAA March Madness Tournament for that given year)|\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['E8', 'E8', 'E8', 'E8', 'F4'], dtype=object)"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "y = df1['POSTSEASON'].values\n",
    "y[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "## Normalize Data \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Data Standardization gives data zero mean and unit variance (technically should be done after train test split )\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/preprocessing/data.py:625: DataConversionWarning: Data with input dtype int64, float64 were all converted to float64 by StandardScaler.\n",
      "  return self.partial_fit(X, y)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/ipykernel_launcher.py:1: DataConversionWarning: Data with input dtype int64, float64 were all converted to float64 by StandardScaler.\n",
      "  \"\"\"Entry point for launching an IPython kernel.\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "array([[-0.43331874, -1.26140173,  0.28034482,  2.74329908, -2.45717765,\n",
       "         0.10027963,  0.94171924, -1.16188145, -1.71391372,  0.12750511,\n",
       "         1.33368704, -0.4942211 , -0.87998988,  0.02784185,  0.00307239,\n",
       "         0.22576157,  1.59744386, -1.12106011, -1.0448016 ,  0.49716104,\n",
       "        -0.6882472 ],\n",
       "       [ 0.40343468,  0.35874728,  0.64758014, -0.90102957,  1.127076  ,\n",
       "         0.39390887,  0.38123706, -1.29466791, -0.03522254, -0.62979797,\n",
       "        -1.31585883, -0.68542235,  0.55458056, -0.07167795, -0.0829545 ,\n",
       "         1.32677295,  0.65081046, -2.369021  ,  0.98050611, -1.14054592,\n",
       "         1.45296631],\n",
       "       [ 0.40343468,  1.33083669,  0.87710222, -1.0788017 ,  1.29403598,\n",
       "         0.72424177, -1.30020946,  0.49794919, -0.16112438, -0.00772758,\n",
       "        -0.27908001,  0.86808783,  1.31063795, -0.03850468, -1.33034432,\n",
       "         1.70643205, -0.29582294,  0.47355659,  1.94493836, -1.14054592,\n",
       "         1.45296631],\n",
       "       [ 0.40343468,  0.68277708,  0.30329703,  0.63966222, -0.04972253,\n",
       "        -0.52368251,  0.63600169, -0.63073565,  0.55231938,  0.50615665,\n",
       "         0.71929959,  1.2743905 ,  0.28317534, -0.07167795, -0.16898138,\n",
       "        -0.91321572,  1.29624232,  0.0922352 ,  0.36969903, -1.14054592,\n",
       "        -0.6882472 ],\n",
       "       [ 0.40343468, -1.58543153, -1.18859646, -0.13068368, -0.87375079,\n",
       "        -1.36786658, -0.17924511,  0.69712887,  0.63625394,  0.34387742,\n",
       "         2.56246194,  0.10328282, -0.49226814, -1.8630343 ,  0.69128747,\n",
       "        -0.30576117, -1.07034117, -1.12106011, -1.88064288,  1.80732661,\n",
       "        -0.6882472 ]])"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X= preprocessing.StandardScaler().fit(X).transform(X)\n",
    "X[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "## Training and Validation \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Split the data into Training and Validation data.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Train set: (44, 21) (44,)\n",
      "Validation set: (12, 21) (12,)\n"
     ]
    }
   ],
   "source": [
    "# We split the X into train and test to find the best k\n",
    "from sklearn.model_selection import train_test_split\n",
    "X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=4)\n",
    "print ('Train set:', X_train.shape,  y_train.shape)\n",
    "print ('Validation set:', X_val.shape,  y_val.shape)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "# Classification \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Now, it is your turn, use the training set to build an accurate model. Then use the validation set  to report the accuracy of the model\n",
    "You should use the following algorithm:\n",
    "- K Nearest Neighbor(KNN)\n",
    "- Decision Tree\n",
    "- Support Vector Machine\n",
    "- Logistic Regression\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# K Nearest Neighbor(KNN)\n",
    "\n",
    "<b>Question  1 </b> Build a KNN model using a value of k equals five, find the accuracy on the validation data (X_val and y_val)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "You can use <code> accuracy_score</cdoe>\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Accuracy on the validation data: 0.67\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/pandas/core/generic.py:6619: SettingWithCopyWarning: \n",
      "A value is trying to be set on a copy of a slice from a DataFrame\n",
      "\n",
      "See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy\n",
      "  return self._update_inplace(result)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/preprocessing/data.py:625: DataConversionWarning: Data with input dtype int64, float64 were all converted to float64 by StandardScaler.\n",
      "  return self.partial_fit(X, y)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/ipykernel_launcher.py:21: DataConversionWarning: Data with input dtype int64, float64 were all converted to float64 by StandardScaler.\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n"
     ]
    }
   ],
   "source": [
    "import itertools\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "from matplotlib.ticker import NullFormatter\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib.ticker as ticker\n",
    "from sklearn import preprocessing\n",
    "%matplotlib inline\n",
    "df = pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ML0101EN-SkillsNetwork/labs/Module%206/cbb.csv')\n",
    "df['windex'] = np.where(df.WAB > 7, 'True', 'False')\n",
    "df1 = df.loc[df['POSTSEASON'].str.contains('F4|S16|E8', na=False)]\n",
    "df1['POSTSEASON'].value_counts()\n",
    "import seaborn as sns\n",
    "df1.groupby(['windex'])['POSTSEASON'].value_counts(normalize=True)\n",
    "df1['windex'].replace(to_replace=['False','True'], value=[0,1],inplace=True)\n",
    "X = df1[['G', 'W', 'ADJOE', 'ADJDE', 'BARTHAG', 'EFG_O', 'EFG_D',\n",
    "       'TOR', 'TORD', 'ORB', 'DRB', 'FTR', 'FTRD', '2P_O', '2P_D', '3P_O',\n",
    "       '3P_D', 'ADJ_T', 'WAB', 'SEED', 'windex']]\n",
    "y = df1['POSTSEASON'].values\n",
    "X= preprocessing.StandardScaler().fit(X).transform(X)\n",
    "from sklearn.model_selection import train_test_split\n",
    "X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=4)\n",
    "#print ('Train set:', X_train.shape,  y_train.shape)\n",
    "#print ('Validation set:', X_val.shape,  y_val.shape)\n",
    "from sklearn.neighbors import KNeighborsClassifier\n",
    "from sklearn.metrics import accuracy_score\n",
    "knn = KNeighborsClassifier(n_neighbors=5)\n",
    "knn.fit(X_train, y_train)\n",
    "y_val_pred = knn.predict(X_val)\n",
    "accuracy = accuracy_score(y_val, y_val_pred)\n",
    "print(f\"Accuracy on the validation data: {accuracy:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question  2</b> Determine and print the accuracy for the first 15 values of k on the validation data:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Accuracy for k=1: 0.33\n",
      "Accuracy for k=2: 0.33\n",
      "Accuracy for k=3: 0.50\n",
      "Accuracy for k=4: 0.58\n",
      "Accuracy for k=5: 0.67\n",
      "Accuracy for k=6: 0.58\n",
      "Accuracy for k=7: 0.58\n",
      "Accuracy for k=8: 0.67\n",
      "Accuracy for k=9: 0.58\n",
      "Accuracy for k=10: 0.58\n",
      "Accuracy for k=11: 0.58\n",
      "Accuracy for k=12: 0.50\n",
      "Accuracy for k=13: 0.58\n",
      "Accuracy for k=14: 0.58\n",
      "Accuracy for k=15: 0.58\n",
      "\n",
      "Accuracies for the first 15 values of k:\n",
      "k=1: 0.33\n",
      "k=2: 0.33\n",
      "k=3: 0.50\n",
      "k=4: 0.58\n",
      "k=5: 0.67\n",
      "k=6: 0.58\n",
      "k=7: 0.58\n",
      "k=8: 0.67\n",
      "k=9: 0.58\n",
      "k=10: 0.58\n",
      "k=11: 0.58\n",
      "k=12: 0.50\n",
      "k=13: 0.58\n",
      "k=14: 0.58\n",
      "k=15: 0.58\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n"
     ]
    }
   ],
   "source": [
    "import numpy as np\n",
    "accuracies = []\n",
    "for k in range(1, 16):    \n",
    "    knn = KNeighborsClassifier(n_neighbors=k)    \n",
    "    knn.fit(X_train, y_train)    \n",
    "    y_val_pred = knn.predict(X_val)    \n",
    "    accuracy = accuracy_score(y_val, y_val_pred)    \n",
    "    accuracies.append(accuracy)    \n",
    "    print(f\"Accuracy for k={k}: {accuracy:.2f}\")\n",
    "print(\"\\nAccuracies for the first 15 values of k:\")\n",
    "for k, accuracy in enumerate(accuracies, start=1):\n",
    "    print(f\"k={k}: {accuracy:.2f}\")\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Decision Tree\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The following lines of code fit a <code>DecisionTreeClassifier</code>:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.tree import DecisionTreeClassifier"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question  3</b> Determine the minumum   value for the parameter <code>max_depth</code> that improves results \n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "\n",
      "Best max_depth: 1 with accuracy: 0.67\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n"
     ]
    }
   ],
   "source": [
    "from sklearn.tree import DecisionTreeClassifier\n",
    "best_accuracy = 0\n",
    "best_max_depth = None\n",
    "for max_depth in range(1, 21):\n",
    "    clf = DecisionTreeClassifier(max_depth=max_depth)    \n",
    "    clf.fit(X_train, y_train)    \n",
    "    y_val_pred = clf.predict(X_val)    \n",
    "    accuracy = accuracy_score(y_val, y_val_pred)    \n",
    "    if accuracy > best_accuracy:\n",
    "        best_accuracy = accuracy\n",
    "        best_max_depth = max_depth    \n",
    "    #print(f\"Accuracy for max_depth={max_depth}: {accuracy:.2f}\")\n",
    "print(f\"\\nBest max_depth: {best_max_depth} with accuracy: {best_accuracy:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Support Vector Machine\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question  4</b> Train the support  vector machine model and determine the accuracy on the validation data for each kernel. Find the kernel (linear, poly, rbf, sigmoid) that provides the best score on the validation data and train a SVM using it.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:35: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=np.finfo(np.float).eps,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:597: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=np.finfo(np.float).eps, copy_X=True, fit_path=True,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:836: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=np.finfo(np.float).eps, copy_X=True, fit_path=True,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:862: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=np.finfo(np.float).eps, positive=False):\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:1097: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  max_n_alphas=1000, n_jobs=None, eps=np.finfo(np.float).eps,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:1344: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  max_n_alphas=1000, n_jobs=None, eps=np.finfo(np.float).eps,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/least_angle.py:1480: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=np.finfo(np.float).eps, copy_X=True, positive=False):\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/randomized_l1.py:152: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  precompute=False, eps=np.finfo(np.float).eps,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/randomized_l1.py:320: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=np.finfo(np.float).eps, random_state=None,\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/randomized_l1.py:580: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  eps=4 * np.finfo(np.float).eps, n_jobs=None,\n"
     ]
    }
   ],
   "source": [
    "from sklearn import svm"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Accuracy for kernel=linear: 0.25\n",
      "Accuracy for kernel=poly: 0.67\n",
      "Accuracy for kernel=rbf: 0.58\n",
      "Accuracy for kernel=sigmoid: 0.50\n",
      "\n",
      "Best kernel: poly with accuracy: 0.67\n",
      "Final model accuracy with kernel=poly: 0.67\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/svm/base.py:196: FutureWarning: The default value of gamma will change from 'auto' to 'scale' in version 0.22 to account better for unscaled features. Set gamma explicitly to 'auto' or 'scale' to avoid this warning.\n",
      "  \"avoid this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/svm/base.py:196: FutureWarning: The default value of gamma will change from 'auto' to 'scale' in version 0.22 to account better for unscaled features. Set gamma explicitly to 'auto' or 'scale' to avoid this warning.\n",
      "  \"avoid this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/svm/base.py:196: FutureWarning: The default value of gamma will change from 'auto' to 'scale' in version 0.22 to account better for unscaled features. Set gamma explicitly to 'auto' or 'scale' to avoid this warning.\n",
      "  \"avoid this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/svm/base.py:196: FutureWarning: The default value of gamma will change from 'auto' to 'scale' in version 0.22 to account better for unscaled features. Set gamma explicitly to 'auto' or 'scale' to avoid this warning.\n",
      "  \"avoid this warning.\", FutureWarning)\n"
     ]
    }
   ],
   "source": [
    "from sklearn import svm\n",
    "from sklearn.svm import SVC\n",
    "from sklearn.metrics import accuracy_score\n",
    "kernels = ['linear', 'poly', 'rbf', 'sigmoid']\n",
    "best_accuracy = 0\n",
    "best_kernel = None\n",
    "for kernel in kernels:    \n",
    "    svm = SVC(kernel=kernel)    \n",
    "    svm.fit(X_train, y_train)    \n",
    "    y_val_pred = svm.predict(X_val)    \n",
    "    accuracy = accuracy_score(y_val, y_val_pred)    \n",
    "    if accuracy > best_accuracy:\n",
    "        best_accuracy = accuracy\n",
    "        best_kernel = kernel    \n",
    "    print(f\"Accuracy for kernel={kernel}: {accuracy:.2f}\")\n",
    "print(f\"\\nBest kernel: {best_kernel} with accuracy: {best_accuracy:.2f}\")\n",
    "final_svm = SVC(kernel=best_kernel)\n",
    "final_svm.fit(X_train, y_train)\n",
    "y_val_pred = final_svm.predict(X_val)\n",
    "final_accuracy = accuracy_score(y_val, y_val_pred)\n",
    "print(f\"Final model accuracy with kernel={best_kernel}: {final_accuracy:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Logistic Regression\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question 5</b> Train a logistic regression model and determine the accuracy of the validation data (set C=0.01)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.linear_model import LogisticRegression"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/logistic.py:460: FutureWarning: Default multi_class will be changed to 'auto' in 0.22. Specify the multi_class option to silence this warning.\n",
      "  \"this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/utils/fixes.py:357: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  if _joblib.__version__ >= LooseVersion('0.12'):\n"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Accuracy on the validation data: 0.67\n"
     ]
    }
   ],
   "source": [
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn.metrics import accuracy_score\n",
    "log_reg = LogisticRegression(C=0.01, solver='lbfgs', max_iter=1000)\n",
    "log_reg.fit(X_train, y_train)\n",
    "y_val_pred = log_reg.predict(X_val)\n",
    "accuracy = accuracy_score(y_val, y_val_pred)\n",
    "print(f\"Accuracy on the validation data: {accuracy:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Model Evaluation using Test set\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.metrics import f1_score\n",
    "# for f1_score please set the average parameter to 'micro'\n",
    "from sklearn.metrics import log_loss"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [],
   "source": [
    "def jaccard_index(predictions, true):\n",
    "    if (len(predictions) == len(true)):\n",
    "        intersect = 0;\n",
    "        for x,y in zip(predictions, true):\n",
    "            if (x == y):\n",
    "                intersect += 1\n",
    "        return intersect / (len(predictions) + len(true) - intersect)\n",
    "    else:\n",
    "        return -1"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Question  5</b> Calculate the  F1 score and Jaccard score for each model from above. Use the Hyperparameter that performed best on the validation data. **For f1_score please set the average parameter to 'micro'.**\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Load Test set for evaluation \n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/IPython/core/interactiveshell.py:3552: FutureWarning: The error_bad_lines argument has been deprecated and will be removed in a future version.\n",
      "\n",
      "\n",
      "  exec(code_obj, self.user_global_ns, self.user_ns)\n"
     ]
    },
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
       "      <th>TEAM</th>\n",
       "      <th>CONF</th>\n",
       "      <th>G</th>\n",
       "      <th>W</th>\n",
       "      <th>ADJOE</th>\n",
       "      <th>ADJDE</th>\n",
       "      <th>BARTHAG</th>\n",
       "      <th>EFG_O</th>\n",
       "      <th>EFG_D</th>\n",
       "      <th>TOR</th>\n",
       "      <th>...</th>\n",
       "      <th>FTRD</th>\n",
       "      <th>2P_O</th>\n",
       "      <th>2P_D</th>\n",
       "      <th>3P_O</th>\n",
       "      <th>3P_D</th>\n",
       "      <th>ADJ_T</th>\n",
       "      <th>WAB</th>\n",
       "      <th>POSTSEASON</th>\n",
       "      <th>SEED</th>\n",
       "      <th>YEAR</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>North Carolina</td>\n",
       "      <td>ACC</td>\n",
       "      <td>40</td>\n",
       "      <td>33</td>\n",
       "      <td>123.3</td>\n",
       "      <td>94.9</td>\n",
       "      <td>0.9531</td>\n",
       "      <td>52.6</td>\n",
       "      <td>48.1</td>\n",
       "      <td>15.4</td>\n",
       "      <td>...</td>\n",
       "      <td>30.4</td>\n",
       "      <td>53.9</td>\n",
       "      <td>44.6</td>\n",
       "      <td>32.7</td>\n",
       "      <td>36.2</td>\n",
       "      <td>71.7</td>\n",
       "      <td>8.6</td>\n",
       "      <td>2ND</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Villanova</td>\n",
       "      <td>BE</td>\n",
       "      <td>40</td>\n",
       "      <td>35</td>\n",
       "      <td>123.1</td>\n",
       "      <td>90.9</td>\n",
       "      <td>0.9703</td>\n",
       "      <td>56.1</td>\n",
       "      <td>46.7</td>\n",
       "      <td>16.3</td>\n",
       "      <td>...</td>\n",
       "      <td>30.0</td>\n",
       "      <td>57.4</td>\n",
       "      <td>44.1</td>\n",
       "      <td>36.2</td>\n",
       "      <td>33.9</td>\n",
       "      <td>66.7</td>\n",
       "      <td>8.9</td>\n",
       "      <td>Champions</td>\n",
       "      <td>2.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Notre Dame</td>\n",
       "      <td>ACC</td>\n",
       "      <td>36</td>\n",
       "      <td>24</td>\n",
       "      <td>118.3</td>\n",
       "      <td>103.3</td>\n",
       "      <td>0.8269</td>\n",
       "      <td>54.0</td>\n",
       "      <td>49.5</td>\n",
       "      <td>15.3</td>\n",
       "      <td>...</td>\n",
       "      <td>26.0</td>\n",
       "      <td>52.9</td>\n",
       "      <td>46.5</td>\n",
       "      <td>37.4</td>\n",
       "      <td>36.9</td>\n",
       "      <td>65.5</td>\n",
       "      <td>2.3</td>\n",
       "      <td>E8</td>\n",
       "      <td>6.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Virginia</td>\n",
       "      <td>ACC</td>\n",
       "      <td>37</td>\n",
       "      <td>29</td>\n",
       "      <td>119.9</td>\n",
       "      <td>91.0</td>\n",
       "      <td>0.9600</td>\n",
       "      <td>54.8</td>\n",
       "      <td>48.4</td>\n",
       "      <td>15.1</td>\n",
       "      <td>...</td>\n",
       "      <td>33.4</td>\n",
       "      <td>52.6</td>\n",
       "      <td>46.3</td>\n",
       "      <td>40.3</td>\n",
       "      <td>34.7</td>\n",
       "      <td>61.9</td>\n",
       "      <td>8.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Kansas</td>\n",
       "      <td>B12</td>\n",
       "      <td>37</td>\n",
       "      <td>32</td>\n",
       "      <td>120.9</td>\n",
       "      <td>90.4</td>\n",
       "      <td>0.9662</td>\n",
       "      <td>55.7</td>\n",
       "      <td>45.1</td>\n",
       "      <td>17.8</td>\n",
       "      <td>...</td>\n",
       "      <td>37.3</td>\n",
       "      <td>52.7</td>\n",
       "      <td>43.4</td>\n",
       "      <td>41.3</td>\n",
       "      <td>32.5</td>\n",
       "      <td>70.1</td>\n",
       "      <td>11.6</td>\n",
       "      <td>E8</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2016</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows × 24 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "             TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  \\\n",
       "0  North Carolina  ACC  40  33  123.3   94.9   0.9531   52.6   48.1  15.4   \n",
       "1       Villanova   BE  40  35  123.1   90.9   0.9703   56.1   46.7  16.3   \n",
       "2      Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3   \n",
       "3        Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1   \n",
       "4          Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8   \n",
       "\n",
       "   ...  FTRD  2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  \n",
       "0  ...  30.4  53.9  44.6  32.7  36.2   71.7   8.6         2ND   1.0  2016  \n",
       "1  ...  30.0  57.4  44.1  36.2  33.9   66.7   8.9   Champions   2.0  2016  \n",
       "2  ...  26.0  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016  \n",
       "3  ...  33.4  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016  \n",
       "4  ...  37.3  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016  \n",
       "\n",
       "[5 rows x 24 columns]"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test_df = pd.read_csv('https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/ML0120ENv3/Dataset/ML0101EN_EDX_skill_up/basketball_train.csv',error_bad_lines=False)\n",
    "test_df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/pandas/core/generic.py:6619: SettingWithCopyWarning: \n",
      "A value is trying to be set on a copy of a slice from a DataFrame\n",
      "\n",
      "See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy\n",
      "  return self._update_inplace(result)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/preprocessing/data.py:625: DataConversionWarning: Data with input dtype int64, float64 were all converted to float64 by StandardScaler.\n",
      "  return self.partial_fit(X, y)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/ipykernel_launcher.py:8: DataConversionWarning: Data with input dtype int64, float64 were all converted to float64 by StandardScaler.\n",
      "  \n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "array([[-4.08074446e-01, -1.10135297e+00,  3.37365934e-01,\n",
       "         2.66479976e+00, -2.46831661e+00,  2.13703245e-01,\n",
       "         9.44090550e-01, -1.19216365e+00, -1.64348924e+00,\n",
       "         1.45405982e-02,  1.29523097e+00, -6.23533182e-01,\n",
       "        -9.31788560e-01,  1.42784371e-01,  1.68876201e-01,\n",
       "         2.84500844e-01,  1.62625961e+00, -8.36649260e-01,\n",
       "        -9.98500539e-01,  4.84319174e-01, -6.77003200e-01],\n",
       "       [ 3.63958290e-01,  3.26326807e-01,  7.03145068e-01,\n",
       "        -7.13778644e-01,  1.07370841e+00,  4.82633172e-01,\n",
       "         4.77498943e-01, -1.32975879e+00, -6.86193316e-02,\n",
       "        -7.35448152e-01, -1.35447914e+00, -8.06829025e-01,\n",
       "         3.41737757e-01,  4.96641291e-02,  9.40576311e-02,\n",
       "         1.37214061e+00,  6.93854620e-01, -2.00860931e+00,\n",
       "         9.80549967e-01, -1.19401460e+00,  1.47709789e+00],\n",
       "       [ 3.63958290e-01,  1.18293467e+00,  9.31757027e-01,\n",
       "        -8.78587347e-01,  1.23870131e+00,  7.85179340e-01,\n",
       "        -9.22275877e-01,  5.27775662e-01, -1.86734575e-01,\n",
       "        -1.19385964e-01, -3.17636057e-01,  6.82449703e-01,\n",
       "         1.01292055e+00,  8.07042098e-02, -9.90811637e-01,\n",
       "         1.74718880e+00, -2.38550367e-01,  6.60855252e-01,\n",
       "         1.92295497e+00, -1.19401460e+00,  1.47709789e+00],\n",
       "       [ 3.63958290e-01,  6.11862762e-01,  3.60227129e-01,\n",
       "         7.14563447e-01, -8.92254236e-02, -3.57772849e-01,\n",
       "         6.89586037e-01, -6.41783067e-01,  4.82585136e-01,\n",
       "         3.89534973e-01,  6.80805434e-01,  1.07195337e+00,\n",
       "         1.00800346e-01,  4.96641291e-02,  1.92390609e-02,\n",
       "        -8.40643737e-01,  1.32958529e+00,  3.02756347e-01,\n",
       "         3.83693465e-01, -1.19401460e+00, -6.77003200e-01],\n",
       "       [ 3.63958290e-01, -1.38688893e+00, -1.12575060e+00,\n",
       "         3.92401673e-04, -9.03545224e-01, -1.13094639e+00,\n",
       "         1.09073363e-02,  7.34168378e-01,  5.61328631e-01,\n",
       "         2.28823098e-01,  2.52408203e+00, -5.07336709e-02,\n",
       "        -5.87592258e-01, -1.62650023e+00,  7.67424763e-01,\n",
       "        -2.40566627e-01, -1.00142717e+00, -8.36649260e-01,\n",
       "        -1.81525154e+00,  1.82698619e+00, -6.77003200e-01]])"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test_df['windex'] = np.where(test_df.WAB > 7, 'True', 'False')\n",
    "test_df1 = test_df[test_df['POSTSEASON'].str.contains('F4|S16|E8', na=False)]\n",
    "test_Feature = test_df1[['G', 'W', 'ADJOE', 'ADJDE', 'BARTHAG', 'EFG_O', 'EFG_D',\n",
    "       'TOR', 'TORD', 'ORB', 'DRB', 'FTR', 'FTRD', '2P_O', '2P_D', '3P_O',\n",
    "       '3P_D', 'ADJ_T', 'WAB', 'SEED', 'windex']]\n",
    "test_Feature['windex'].replace(to_replace=['False','True'], value=[0,1],inplace=True)\n",
    "test_X=test_Feature\n",
    "test_X= preprocessing.StandardScaler().fit(test_X).transform(test_X)\n",
    "test_X[0:5]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['E8', 'E8', 'E8', 'E8', 'F4'], dtype=object)"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test_y = test_df1['POSTSEASON'].values\n",
    "test_y[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "KNN\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "KNN Model - F1 Score: 0.67, Jaccard Index: 0.50\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:907: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  self._y = np.empty(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/neighbors/base.py:442: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  old_joblib = LooseVersion(joblib_version) < LooseVersion('0.12')\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/tree/tree.py:149: DeprecationWarning: `np.int` is a deprecated alias for the builtin `int`. To silence this warning, use `int` by itself. Doing this will not modify any behavior and is safe. When replacing `np.int`, you may wish to use e.g. `np.int64` or `np.int32` to specify the precision. If you wish to review your current use, check the release note link for additional information.\n",
      "Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations\n",
      "  y_encoded = np.zeros(y.shape, dtype=np.int)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/svm/base.py:196: FutureWarning: The default value of gamma will change from 'auto' to 'scale' in version 0.22 to account better for unscaled features. Set gamma explicitly to 'auto' or 'scale' to avoid this warning.\n",
      "  \"avoid this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/logistic.py:460: FutureWarning: Default multi_class will be changed to 'auto' in 0.22. Specify the multi_class option to silence this warning.\n",
      "  \"this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/utils/fixes.py:357: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  if _joblib.__version__ >= LooseVersion('0.12'):\n"
     ]
    }
   ],
   "source": [
    "from sklearn.neighbors import KNeighborsClassifier\n",
    "from sklearn.tree import DecisionTreeClassifier\n",
    "from sklearn.svm import SVC\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn.metrics import f1_score, log_loss\n",
    "from sklearn.metrics import f1_score\n",
    "from sklearn.metrics import log_loss\n",
    "def jaccard_index(predictions, true):\n",
    "    if (len(predictions) == len(true)):\n",
    "        intersect = 0;\n",
    "        for x,y in zip(predictions, true):\n",
    "            if (x == y):\n",
    "                intersect += 1\n",
    "        return intersect / (len(predictions) + len(true) - intersect)\n",
    "    else:\n",
    "        return -1\n",
    "best_knn = KNeighborsClassifier(n_neighbors=5)\n",
    "best_knn.fit(X_train, y_train)\n",
    "y_val_pred_knn = best_knn.predict(X_val)\n",
    "best_max_depth = 5\n",
    "best_dt = DecisionTreeClassifier(max_depth=best_max_depth)\n",
    "best_dt.fit(X_train, y_train)\n",
    "y_val_pred_dt = best_dt.predict(X_val)\n",
    "\n",
    "best_kernel = 'rbf'  \n",
    "best_svm = SVC(kernel=best_kernel)\n",
    "best_svm.fit(X_train, y_train)\n",
    "y_val_pred_svm = best_svm.predict(X_val)\n",
    "\n",
    "log_reg = LogisticRegression(C=0.01, solver='lbfgs', max_iter=1000)\n",
    "log_reg.fit(X_train, y_train)\n",
    "y_val_pred_log_reg = log_reg.predict(X_val)\n",
    "\n",
    "f1_knn = f1_score(y_val, y_val_pred_knn, average='micro')\n",
    "jaccard_knn = jaccard_index(y_val_pred_knn, y_val)\n",
    "\n",
    "f1_dt = f1_score(y_val, y_val_pred_dt, average='micro')\n",
    "jaccard_dt = jaccard_index(y_val_pred_dt, y_val)\n",
    "\n",
    "f1_svm = f1_score(y_val, y_val_pred_svm, average='micro')\n",
    "jaccard_svm = jaccard_index(y_val_pred_svm, y_val)\n",
    "\n",
    "f1_log_reg = f1_score(y_val, y_val_pred_log_reg, average='micro')\n",
    "jaccard_log_reg = jaccard_index(y_val_pred_log_reg, y_val)\n",
    "print(f\"KNN Model - F1 Score: {f1_knn:.2f}, Jaccard Index: {jaccard_knn:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Decision Tree\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Decision Tree Model - F1 Score: 0.42, Jaccard Index: 0.26\n"
     ]
    }
   ],
   "source": [
    "print(f\"Decision Tree Model - F1 Score: {f1_dt:.2f}, Jaccard Index: {jaccard_dt:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "SVM\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "SVM Model - F1 Score: 0.58, Jaccard Index: 0.41\n"
     ]
    }
   ],
   "source": [
    "print(f\"SVM Model - F1 Score: {f1_svm:.2f}, Jaccard Index: {jaccard_svm:.2f}\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Logistic Regression\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Logistic Regression Model - F1 Score: 0.67, Jaccard Index: 0.50\n",
      "loss is: 0.923299518240681\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/linear_model/logistic.py:460: FutureWarning: Default multi_class will be changed to 'auto' in 0.22. Specify the multi_class option to silence this warning.\n",
      "  \"this warning.\", FutureWarning)\n",
      "/home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/sklearn/utils/fixes.py:357: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n",
      "  if _joblib.__version__ >= LooseVersion('0.12'):\n"
     ]
    }
   ],
   "source": [
    "print(f\"Logistic Regression Model - F1 Score: {f1_log_reg:.2f}, Jaccard Index: {jaccard_log_reg:.2f}\")\n",
    "log_reg = LogisticRegression(C=0.01, solver='lbfgs', max_iter=1000)\n",
    "log_reg.fit(X_train, y_train)\n",
    "y_val_pred_log_reg = log_reg.predict(X_val)\n",
    "y_val_pred_proba_log_reg = log_reg.predict_proba(X_val)\n",
    "log_loss_log_reg = log_loss(y_val, y_val_pred_proba_log_reg)\n",
    "print(\"loss is:\",log_loss_log_reg)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Report\n",
    "You should be able to report the accuracy of the built model using different evaluation metrics:\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "| Algorithm          | Accuracy | Jaccard  | F1-score  | LogLoss |\n",
    "|--------------------|----------|----------|-----------|---------|\n",
    "| KNN                |  0.67    |  0.50    |  0.67     | NA      |\n",
    "| Decision Tree      |  0.67    |  0.26    |  0.42     | NA      |\n",
    "| SVM                |  0.67    |  0.41    |  0.58     | NA      |\n",
    "| LogisticRegression |  0.67    |  0.50    |  0.67     | 0.923299518  |\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Something to keep in mind when creating models to predict the results of basketball tournaments or sports in general is that is quite hard due to so many factors influencing the game. Even in sports betting an accuracy of 55% and over is considered good as it indicates profits.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "<h2>Want to learn more?</h2>\n",
    "\n",
    "IBM SPSS Modeler is a comprehensive analytics platform that has many machine learning algorithms. It has been designed to bring predictive intelligence to decisions made by individuals, by groups, by systems – by your enterprise as a whole. A free trial is available through this course, available here: <a href=\"https://www.ibm.com/analytics/spss-statistics-software?utm_source=Exinfluencer&utm_content=000026UJ&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork1047-2023-01-01&utm_medium=Exinfluencer&utm_term=10006555\">SPSS Modeler</a>\n",
    "\n",
    "Also, you can use Watson Studio to run these notebooks faster with bigger datasets. Watson Studio is IBM's leading cloud solution for data scientists, built by data scientists. With Jupyter notebooks, RStudio, Apache Spark and popular libraries pre-packaged in the cloud, Watson Studio enables data scientists to collaborate on their projects without having to install anything. Join the fast-growing community of Watson Studio users today with a free account at <a href=\"https://www.ibm.com/cloud/watson-studio?utm_source=Exinfluencer&utm_content=000026UJ&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork1047-2023-01-01&utm_medium=Exinfluencer&utm_term=10006555\">Watson Studio</a>\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Thank you for completing this lab!\n",
    "\n",
    "\n",
    "## Author\n",
    "\n",
    "Saeed Aghabozorgi\n",
    "\n",
    "\n",
    "### Other Contributors\n",
    "\n",
    "<a href=\"https://www.linkedin.com/in/joseph-s-50398b136/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork1047-2023-01-01\">Joseph Santarcangelo</a>\n",
    "\n",
    "\n",
    "\n",
    "\n",
    "## Change Log\n",
    "\n",
    "\n",
    "|  Date (YYYY-MM-DD) |  Version | Changed By  |  Change Description |\n",
    "|---|---|---|---|\n",
    "|2021-04-03   | 2.1  | Malika Singla| Updated the Report accuracy |\n",
    "| 2020-08-27  | 2.0  | Lavanya  |  Moved lab to course repo in GitLab |\n",
    "|   |   |   |   |\n",
    "|   |   |   |   |\n",
    "\n",
    "\n",
    "## <h3 align=\"center\"> © IBM Corporation 2020. All rights reserved. <h3/>\n"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python",
   "language": "python",
   "name": "conda-env-python-py"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.12"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}

