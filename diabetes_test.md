{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "fd54d589",
   "metadata": {
    "_cell_guid": "b1076dfc-b9ad-4769-8c92-a6c4dae69d19",
    "_uuid": "8f2839f25d086af736a60e9eeb907d3b93b6e0e5",
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:55.819207Z",
     "iopub.status.busy": "2024-04-20T22:40:55.818733Z",
     "iopub.status.idle": "2024-04-20T22:40:58.310870Z",
     "shell.execute_reply": "2024-04-20T22:40:58.309629Z"
    },
    "papermill": {
     "duration": 2.505902,
     "end_time": "2024-04-20T22:40:58.313344",
     "exception": false,
     "start_time": "2024-04-20T22:40:55.807442",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "/kaggle/input/diabetes-dataset/diabetes.csv\n"
     ]
    }
   ],
   "source": [
    "# This Python 3 environment comes with many helpful analytics libraries installed\n",
    "# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python\n",
    "# For example, here's several helpful packages to load\n",
    "\n",
    "import numpy as np # linear algebra\n",
    "import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "from scipy import stats\n",
    "# Input data files are available in the read-only \"../input/\" directory\n",
    "# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory\n",
    "\n",
    "import os\n",
    "for dirname, _, filenames in os.walk('/kaggle/input'):\n",
    "    for filename in filenames:\n",
    "        print(os.path.join(dirname, filename))\n",
    "\n",
    "# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using \"Save & Run All\" \n",
    "# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "0bb9d1bd",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:58.332333Z",
     "iopub.status.busy": "2024-04-20T22:40:58.331759Z",
     "iopub.status.idle": "2024-04-20T22:40:58.357285Z",
     "shell.execute_reply": "2024-04-20T22:40:58.355955Z"
    },
    "papermill": {
     "duration": 0.038445,
     "end_time": "2024-04-20T22:40:58.360355",
     "exception": false,
     "start_time": "2024-04-20T22:40:58.321910",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "diabetes = pd.read_csv('/kaggle/input/diabetes-dataset/diabetes.csv')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "f231a101",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:58.379199Z",
     "iopub.status.busy": "2024-04-20T22:40:58.378564Z",
     "iopub.status.idle": "2024-04-20T22:40:58.407238Z",
     "shell.execute_reply": "2024-04-20T22:40:58.405898Z"
    },
    "papermill": {
     "duration": 0.041681,
     "end_time": "2024-04-20T22:40:58.410398",
     "exception": false,
     "start_time": "2024-04-20T22:40:58.368717",
     "status": "completed"
    },
    "tags": []
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
       "      <th>Pregnancies</th>\n",
       "      <th>Glucose</th>\n",
       "      <th>BloodPressure</th>\n",
       "      <th>SkinThickness</th>\n",
       "      <th>Insulin</th>\n",
       "      <th>BMI</th>\n",
       "      <th>DiabetesPedigreeFunction</th>\n",
       "      <th>Age</th>\n",
       "      <th>Outcome</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>6</td>\n",
       "      <td>148</td>\n",
       "      <td>72</td>\n",
       "      <td>35</td>\n",
       "      <td>0</td>\n",
       "      <td>33.6</td>\n",
       "      <td>0.627</td>\n",
       "      <td>50</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1</td>\n",
       "      <td>85</td>\n",
       "      <td>66</td>\n",
       "      <td>29</td>\n",
       "      <td>0</td>\n",
       "      <td>26.6</td>\n",
       "      <td>0.351</td>\n",
       "      <td>31</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>8</td>\n",
       "      <td>183</td>\n",
       "      <td>64</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>23.3</td>\n",
       "      <td>0.672</td>\n",
       "      <td>32</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1</td>\n",
       "      <td>89</td>\n",
       "      <td>66</td>\n",
       "      <td>23</td>\n",
       "      <td>94</td>\n",
       "      <td>28.1</td>\n",
       "      <td>0.167</td>\n",
       "      <td>21</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>0</td>\n",
       "      <td>137</td>\n",
       "      <td>40</td>\n",
       "      <td>35</td>\n",
       "      <td>168</td>\n",
       "      <td>43.1</td>\n",
       "      <td>2.288</td>\n",
       "      <td>33</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Pregnancies  Glucose  BloodPressure  SkinThickness  Insulin   BMI  \\\n",
       "0            6      148             72             35        0  33.6   \n",
       "1            1       85             66             29        0  26.6   \n",
       "2            8      183             64              0        0  23.3   \n",
       "3            1       89             66             23       94  28.1   \n",
       "4            0      137             40             35      168  43.1   \n",
       "\n",
       "   DiabetesPedigreeFunction  Age  Outcome  \n",
       "0                     0.627   50        1  \n",
       "1                     0.351   31        0  \n",
       "2                     0.672   32        1  \n",
       "3                     0.167   21        0  \n",
       "4                     2.288   33        1  "
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "diabetes.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "a7cfa533",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:58.430414Z",
     "iopub.status.busy": "2024-04-20T22:40:58.430039Z",
     "iopub.status.idle": "2024-04-20T22:40:58.437250Z",
     "shell.execute_reply": "2024-04-20T22:40:58.436037Z"
    },
    "papermill": {
     "duration": 0.020329,
     "end_time": "2024-04-20T22:40:58.439697",
     "exception": false,
     "start_time": "2024-04-20T22:40:58.419368",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['Pregnancies', 'Glucose', 'BloodPressure', 'SkinThickness', 'Insulin',\n",
       "       'BMI', 'DiabetesPedigreeFunction', 'Age', 'Outcome'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "diabetes.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "977f5568",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:58.458764Z",
     "iopub.status.busy": "2024-04-20T22:40:58.458361Z",
     "iopub.status.idle": "2024-04-20T22:40:59.076112Z",
     "shell.execute_reply": "2024-04-20T22:40:59.074782Z"
    },
    "papermill": {
     "duration": 0.631885,
     "end_time": "2024-04-20T22:40:59.080095",
     "exception": false,
     "start_time": "2024-04-20T22:40:58.448210",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "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",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.regplot(x='Glucose',y='DiabetesPedigreeFunction', data = diabetes)\n",
    "plt.ylim(0,)\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "id": "f1f0f1f7",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.102411Z",
     "iopub.status.busy": "2024-04-20T22:40:59.101925Z",
     "iopub.status.idle": "2024-04-20T22:40:59.112149Z",
     "shell.execute_reply": "2024-04-20T22:40:59.110779Z"
    },
    "papermill": {
     "duration": 0.02463,
     "end_time": "2024-04-20T22:40:59.114823",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.090193",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "pearson_cof,p_value = stats.pearsonr(diabetes['Glucose'],diabetes['DiabetesPedigreeFunction'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "id": "a5b0202c",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.137888Z",
     "iopub.status.busy": "2024-04-20T22:40:59.136805Z",
     "iopub.status.idle": "2024-04-20T22:40:59.144240Z",
     "shell.execute_reply": "2024-04-20T22:40:59.142640Z"
    },
    "papermill": {
     "duration": 0.02192,
     "end_time": "2024-04-20T22:40:59.146836",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.124916",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "correlation is:  0.1373372998283707\n",
      "p value is : 0.00013458781437157466\n"
     ]
    }
   ],
   "source": [
    "print('correlation is: ', pearson_cof)\n",
    "print('p value is :', p_value)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "e62d5374",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.169991Z",
     "iopub.status.busy": "2024-04-20T22:40:59.169051Z",
     "iopub.status.idle": "2024-04-20T22:40:59.187335Z",
     "shell.execute_reply": "2024-04-20T22:40:59.186024Z"
    },
    "papermill": {
     "duration": 0.033252,
     "end_time": "2024-04-20T22:40:59.190220",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.156968",
     "status": "completed"
    },
    "tags": []
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
       "      <th>DiabetesPedigreeFunction</th>\n",
       "      <th>Glucose</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0.627</td>\n",
       "      <td>148</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>0.351</td>\n",
       "      <td>85</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>0.672</td>\n",
       "      <td>183</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>0.167</td>\n",
       "      <td>89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>2.288</td>\n",
       "      <td>137</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   DiabetesPedigreeFunction  Glucose\n",
       "0                     0.627      148\n",
       "1                     0.351       85\n",
       "2                     0.672      183\n",
       "3                     0.167       89\n",
       "4                     2.288      137"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "diabetes[['DiabetesPedigreeFunction', 'Glucose']].head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "id": "501dc09d",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.214489Z",
     "iopub.status.busy": "2024-04-20T22:40:59.213979Z",
     "iopub.status.idle": "2024-04-20T22:40:59.219279Z",
     "shell.execute_reply": "2024-04-20T22:40:59.217972Z"
    },
    "papermill": {
     "duration": 0.020989,
     "end_time": "2024-04-20T22:40:59.222037",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.201048",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "w = 0.0031\n",
    "b = 0"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "id": "19386b8f",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.248619Z",
     "iopub.status.busy": "2024-04-20T22:40:59.247694Z",
     "iopub.status.idle": "2024-04-20T22:40:59.253932Z",
     "shell.execute_reply": "2024-04-20T22:40:59.252566Z"
    },
    "papermill": {
     "duration": 0.022419,
     "end_time": "2024-04-20T22:40:59.257189",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.234770",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "x= np.linspace(0, diabetes['Glucose'].max(), 100)\n",
    "y =w*x+b"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "id": "6f950c82",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.282426Z",
     "iopub.status.busy": "2024-04-20T22:40:59.281292Z",
     "iopub.status.idle": "2024-04-20T22:40:59.610601Z",
     "shell.execute_reply": "2024-04-20T22:40:59.609260Z"
    },
    "papermill": {
     "duration": 0.345342,
     "end_time": "2024-04-20T22:40:59.614220",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.268878",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAjcAAAGwCAYAAABVdURTAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuNSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/xnp5ZAAAACXBIWXMAAA9hAAAPYQGoP6dpAACMyklEQVR4nO3dd3xUVdoH8N+dZNJIb6QQUkhEOqEKKAIqyLoC4rossgsqsiugqNiWteu+YgF1l0VYVwSxoOsuRXGXFZEivSShGxLSKAmEVNLL3PePcIcpt8+dmTuT5/t+/LzL1DO5M/c895znPIdhWZYFIYQQQoiXMLi7AYQQQgghWqLghhBCCCFehYIbQgghhHgVCm4IIYQQ4lUouCGEEEKIV6HghhBCCCFehYIbQgghhHgVX3c3wNVMJhMuXryIkJAQMAzj7uYQQgghRAaWZXH16lUkJCTAYBAfm+l0wc3FixeRlJTk7mYQQgghRIVz586hW7duoo/pdMFNSEgIgI4/TmhoqJtbQwghhBA5amtrkZSUZO7HxXS64IabigoNDaXghhBCCPEwclJKKKGYEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXcWtws3jxYgwdOhQhISGIjY3FlClTkJubK/qcNWvWgGEYq/8CAgJc1GJCCCGE6J1bg5udO3di/vz52L9/P7Zu3YrW1laMHz8e9fX1os8LDQ1FaWmp+b/i4mIXtZgQQggheufrzjffsmWL1b/XrFmD2NhYHDlyBKNHjxZ8HsMwiIuLc3bzCCGEEOKBdJVzU1NTAwCIjIwUfVxdXR2Sk5ORlJSEyZMn4+TJk4KPbW5uRm1trdV/hBBCCPFeugluTCYTnnjiCYwaNQp9+/YVfFzPnj3x8ccfY9OmTfjss89gMpkwcuRInD9/nvfxixcvRlhYmPm/pKQkZ30EQgghhOgAw7Is6+5GAMDcuXPx3//+F7t370a3bt1kP6+1tRW9evXC9OnT8frrr9vd39zcjObmZvO/a2trkZSUhJqaGoSGhmrSdkIIIYQ4V21tLcLCwmT1327NueE8+uij2Lx5M3bt2qUosAEAo9GIzMxM5Ofn897v7+8Pf39/LZpJCCGEEA/g1mkplmXx6KOPYsOGDfjxxx+Rmpqq+DXa29tx/PhxxMfHO6GFhBBCCPE0bh25mT9/Pr744gts2rQJISEhKCsrAwCEhYUhMDAQADBz5kwkJiZi8eLFAIDXXnsNN910E9LT01FdXY133nkHxcXFePjhh932OQghhBCiH24NblasWAEAGDNmjNXtq1evxgMPPAAAKCkpgcFwfYCpqqoKc+bMQVlZGSIiIjB48GDs3bsXvXv3dlWzCSGEEKJjukkodhUlCUmEEEII0QePSygmhBBCiLCC8joUVzYgJaoLUqO7uLs5ukfBDSGEEKJT1Q0tWLAuB7vyys23jc6IwbLpmQgLMrqxZfqmmyJ+hBBCCLG2YF0O9uRfsbptT/4VPLYu200t8gwU3BBCCCE6VFBeh1155Wi3SY1tZ1nsyitH4RXxTaY7MwpuCCGEEB0qrmwQvb+ogoIbIRTcEEIIITqUHBkken9KFCUWC6HghhBCCNGhtJhgjM6IgQ/DWN3uwzAYnRFDq6ZEUHBDCCGE6NSy6ZkYlR5tdduo9Ggsm57pphZ5BloKTgghhOhUWJARa2cPQ+GVehRV1FOdG5kouCGEEEJ0LjWagholaFqKEEIIIV6FghtCCCGEeBWaliKEEEKIKnrd84qCG0IIIYQoovc9r2haihBCCCGK6H3PKwpuCCGEECKbJ+x5RcENIYQQQmTzhD2vKLghhBBCiGyesOcVBTeEEEIEFZTXYXvuZV1MNRB98IQ9r2i1FCGEEDt6Xw1D3GvZ9Ew8ti7b6vuhpz2vGJa1yQjycrW1tQgLC0NNTQ1CQ0Pd3RxCCNGlmasOYk/+FaukUR+Gwaj0aKydPcyNLSN64so9r5T03zRyQwghxAq3GsaW5WoYPUw9EPfT655XlHNDCCHEiieshiFEDAU3hBBCrHjCahhCxFBwQwghxIonrIYhRAwFN4QQQuwsm56JUenRVrfpaTUMIWIooZgQQoidsCAj1s4e5tLVMIRohYIbQgghgvS6GoYQMTQtRQghhBCvQsENIYQQQrwKBTeEEEII8SoU3BBCCCHEq1BwQwghhBCvQsENIYQQQrwKBTeEEEII8SoU3BBCCCHEq1BwQwghhBCvQsENIYQQQrwKBTeEEEII8Sq0txQhhBDiZAXldSiubKANSF2EghtCCCHESaobWrBgXQ525ZWbbxudEYNl0zMRFmR0Y8u8G01LEUIIIU6yYF0O9uRfsbptT/4VPLYu200t6hwouCGEEEKcoKC8DrvyytHOsla3t7MsduWVo/BKvZta5v0ouCGEEEKcoLiyQfT+ogoKbpyFghtCCCHECZIjg0TvT4mixGJnoeCGEEIIcYK0mGCMzoiBD8NY3e7DMBidEUOrppyIghtCCCHESZZNz8So9Gir20alR2PZ9Ew3tahzoKXghBBCiJOEBRmxdvYwFF6pR1FFPdW5cREKbgghhBAnS42moMaVaFqKEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV7FrcHN4sWLMXToUISEhCA2NhZTpkxBbm6u5PO+/vpr3HjjjQgICEC/fv3wn//8xwWtJYQQQogncGtws3PnTsyfPx/79+/H1q1b0draivHjx6O+vl7wOXv37sX06dMxe/ZsZGdnY8qUKZgyZQpOnDjhwpYTQgghRK8YlmVZdzeCU15ejtjYWOzcuROjR4/mfcy0adNQX1+PzZs3m2+76aabMHDgQKxcuVLyPWpraxEWFoaamhqEhoZq1nZCCCGEOI+S/ltXOTc1NTUAgMjISMHH7Nu3D7fffrvVbRMmTMC+fft4H9/c3Iza2lqr/wghhBDivXQT3JhMJjzxxBMYNWoU+vbtK/i4srIydO3a1eq2rl27oqysjPfxixcvRlhYmPm/pKQkTdtNCCGEEH3RTXAzf/58nDhxAl9++aWmr7to0SLU1NSY/zt37pymr08IIYQQffF1dwMA4NFHH8XmzZuxa9cudOvWTfSxcXFxuHTpktVtly5dQlxcHO/j/f394e/vr1lbCSGEEKJvbh25YVkWjz76KDZs2IAff/wRqampks8ZMWIEtm3bZnXb1q1bMWLECGc1kxBCCCEexK0jN/Pnz8cXX3yBTZs2ISQkxJw3ExYWhsDAQADAzJkzkZiYiMWLFwMAHn/8cdx6661YunQp7rrrLnz55Zc4fPgwPvzwQ7d9DkIIIYToh1tHblasWIGamhqMGTMG8fHx5v+++uor82NKSkpQWlpq/vfIkSPxxRdf4MMPP8SAAQPwr3/9Cxs3bhRNQiaEEEJI5+FQnZuWlhZcvnwZJpPJ6vbu3bs73DBnoTo3hBBCiOdR0n+rmpbKy8vDQw89hL1791rdzrIsGIZBe3u7mpclhBBCCHGYquDmgQcegK+vLzZv3oz4+HgwDKN1uwghhBBCVFEV3OTk5ODIkSO48cYbtW4PIYQQQohDVCUU9+7dG1euXNG6LYQQQgghDlMV3Lz11lt49tlnsWPHDlRUVNDeTYQQQogTFZTXYXvuZRReqXd3UzyCqtVSBkNHTGSba+MJCcW0WooQQoinqG5owYJ1OdiVV26+bXRGDJZNz0RYkNGNLXM9p6+W2r59u6qGEUIIIUS+BetysCffOg1kT/4VPLYuG2tnD3NTq/RPVXBz6623at0OQgghhFgoKK+zGrHhtLMsduWVo/BKPVKju7ihZfqnevuF6upqrFq1CqdPnwYA9OnTBw899BDCwsI0axwhhBDSWRVXNojeX1RBwY0QVQnFhw8fRo8ePfDee++hsrISlZWVePfdd9GjRw9kZWVp3UZCCCGk00mODBK9PyWKAhshqoKbJ598EpMmTUJRURHWr1+P9evXo7CwEL/85S/xxBNPaNxEQgghpPNJiwnG6IwY+Ngs3vFhGIzOiKFRGxGqVksFBgYiOzvbrojfqVOnMGTIEDQ0iA+luROtliKEEOIpahpa8di6bFotBReslgoNDUVJSYldcHPu3DmEhISoeUlCCCGE2AgLMmLt7GEovFKPoop6pER1oREbGVQFN9OmTcPs2bOxZMkSjBw5EgCwZ88ePPPMM5g+fbqmDSSEEEI6u9RoCmqUUBXcLFmyBAzDYObMmWhrawMAGI1GzJ07F2+++aamDSSEEEIIUUJVzg2noaEBZ8+eBQD06NEDQUHimd16QDk3hBBCiOdxes4NJygoCP369XPkJQghhBBCNCU7uJk6dSrWrFmD0NBQTJ06VfSx69evd7hhhBBCCCFqyA5uwsLCzBtlhoaG2m2aSQghhBCiBw7l3HgiyrkhhBD3KiivQ3FlAy1rJoo4Pedm3LhxWL9+PcLDw+3eeMqUKfjxxx/VvCwhhBAvVt3QggXrcqggHXE6Vdsv7NixAy0tLXa3NzU14aeffnK4UYQQQrzPgnU52JN/xeq2PflX8Ni6bDe1iHgrRSM3x44dM//vU6dOoayszPzv9vZ2bNmyBYmJidq1jhBCiFcoKK+zGrHhtLMsduWVo/AK7XBNtKMouBk4cCAYhgHDMBg3bpzd/YGBgVi2bJlmjSOEEOIdiivF9xwsqqDghmhHUXBTWFgIlmWRlpaGgwcPIiYmxnyfn58fYmNj4ePjo3kjCSGEeLbkSPEirylRFNgQ7SgKbpKTkwEAJpPJKY0hhBDindJigjE6IwZ78q+g3WKRrg/DYFR6NI3aEE2pSihevHgxPv74Y7vbP/74Y7z11lsON4oQQrxZQXkdtudeRuGVenc3xaWWTc/EqPRoq9tGpUdj2fRMN7WIeCtVdW5SUlLwxRdfmHcE5xw4cAC/+c1vUFhYqFkDtUZ1bggh7kJLoTsUXqlHUUU91bkhiijpv1WN3JSVlSE+Pt7u9piYGJSWlqp5SUII8Xq0FLpDanQXjO0ZS4ENcRpVwU1SUhL27Nljd/uePXuQkJDgcKMIIcTbcEuh220Gyy2XQhNCtKGqQvGcOXPwxBNPoLW11bwkfNu2bXj22Wfx1FNPadpAQgjxBrQUmhDXURXcPPPMM6ioqMC8efPMlYoDAgLw3HPPYdGiRZo2kBCiL87aF8jb9xuipdCEuI5DG2fW1dXh9OnTCAwMREZGBvz9/bVsm1NQQjEh6jgrGbYzJdnOXHVQcCn02tnD3NgyQvTP6QnFnODgYAwdOhR9+/b1iMCGEKKes5JhO1OSLS2FJsQ1VE1L1dfX480338S2bdtw+fJlu6J+BQUFmjSOEKIPztoXqLPtNxQWZMTa2cNoKTQhTqYquHn44Yexc+dO/O53v0N8fDwYhtG6XYQQHXFWMmxnTbJNjaaghhBnUhXc/Pe//8V3332HUaNGad0eQogOOSsZlpJsCSHOoCrnJiIiApGRkVq3hRCiU9y+QD42o7Q+DIPRGTGqRyGc9bqEkM5NVXDz+uuv46WXXkJDg/iQMiHEezgrGZaSbAkhWlO1FDwzMxNnz54Fy7JISUmB0Wi9XDMrK0uzBmqNloIT4hhnJcNSki0hRIyS/ltVzs2UKVPUPI0Q4gWclQxLSbaEEK04VMTPE9HIDSGEEOJ5XFbEjxBCCCFEb1RNSxkMBtHaNu3t7aobRAghhBDiCFXBzYYNG6z+3draiuzsbHzyySd49dVXNWkYIYQQQogamubcfPHFF/jqq6+wadMmrV5Sc5RzQwjxRt6+qzohTl8tJeSmm27C73//ey1fkhBCiIjOtKs6IXJpllDc2NiIv/71r0hMTNTqJQkhhEjoTLuqEyKXqpGbiIgIq4RilmVx9epVBAUF4bPPPtOscYQQQoR1tl3VCZFLVXDz/vvvW/3bYDAgJiYGw4cPR0REhBbtIoQQIqGz7qpOiBRFwc3HH3+MGTNmYNasWc5qDyGEEJloV3VC+CnKuZkzZw5qamrM/05ISEBRUZHWbSKEECID7apOCD9FwY3tqvGrV6/CZDJp2iBCCCHy0a7qhNjTdCk4IYQQ1woLMmLt7GG0qzohFhQFNwzDWK2Ssv03IYQQ96Bd1Qm5TlFww7IsbrjhBnNAU1dXh8zMTBgM1rNblZWV2rWQEEIIIUQBRcHN6tWrndUOQgghhBBNKApuaAk4IYQQQvRO9fYL1dXV+Oijj7Bo0SLzNFRWVhYuXLigWeMIIYQQQpRSFdwcO3YMN9xwA9566y0sWbIE1dXVAID169dj0aJFsl9n165duPvuu5GQkACGYbBx40bRx+/YscOcxGz5X1lZmZqPQQghhBAvpCq4WbhwIR544AHk5eUhICDAfPsvfvEL7Nq1S/br1NfXY8CAAVi+fLmi98/NzUVpaan5v9jYWEXPJ4QQQoj3UlXn5tChQ/j73/9ud3tiYqKiUZSJEydi4sSJit8/NjYW4eHhsh7b3NyM5uZm879ra2sVvx8hhBBCPIeqkRt/f3/eIOHMmTOIiYlxuFFSBg4ciPj4eNxxxx3Ys2eP6GMXL16MsLAw839JSUlObx8hhIgpKK/D9tzLKLxS7+6mEOKVVAU3kyZNwmuvvYbW1lYAHcX8SkpK8Nxzz+Hee+/VtIGW4uPjsXLlSvz73//Gv//9byQlJWHMmDHIysoSfM6iRYtQU1Nj/u/cuXNOax8hhIipbmjBzFUHMW7pTjy4+hDGLtmBmasOoqah1d1NI8SrMKzthlEy1NTU4Fe/+hUOHz6Mq1evIiEhAWVlZRgxYgT+85//oEsX5VUyGYbBhg0bMGXKFEXPu/XWW9G9e3d8+umnsh5fW1uLsLAw1NTUIDQ0VHE7CSFErZmrDmJP/hW0W5x2fRgGo9KjsXb2MDe2jHiqgvI6FFc2dIptN5T036pybsLCwrB161bs3r0bx44dQ11dHQYNGoTbb79dVYMdMWzYMOzevdvl70sIIUoUlNdhV1653e3tLItdeeUovFLv9Z0T0U51QwsWrMux+k6NzojBsumZCAsyurFl+uDQxpk333wzhgwZAn9/f7ftMZWTk4P4+Hi3vDchhMhVXNkgen9RBQU3RL4F63KwJ/+K1W178q/gsXXZNAoIlTk3JpMJr7/+OhITExEcHIzCwkIAwIsvvohVq1bJfp26ujrk5OQgJycHAFBYWIicnByUlJQA6MiXmTlzpvnx77//PjZt2oT8/HycOHECTzzxBH788UfMnz9fzccghBCXSY4MEr0/JcpzAhtKiHYvbhSw3SarxHIUsLNTFdz8+c9/xpo1a/D222/Dz8/PfHvfvn3x0UcfyX6dw4cPIzMzE5mZmQA66udkZmbipZdeAgCUlpaaAx0AaGlpwVNPPYV+/frh1ltvxdGjR/HDDz/gtttuU/MxCCHEZdJigjE6IwY+NqPcPgyD0RkxHjFqQwnR+iBnFLCzU5VQnJ6ejr///e+47bbbEBISgqNHjyItLQ0///wzRowYgaqqKme0VROUUEwIcZeahlY8ti7bY/MkKCFaHwrK6zBu6U7B+7c/PcYjgmWlnJ5QfOHCBaSnp9vdbjKZzMvDCSGEWAsLMmLt7GEovFKPoop6j1rhQgnR+sGNAgoFmnQcVE5L9e7dGz/99JPd7f/617/MU0yEEEL4pUZ3wdiesR7VCdFUiL4sm56JUenRVreNSo/GsunUBwMqR25eeuklzJo1CxcuXIDJZML69euRm5uLtWvXYvPmzVq3kRBCnK4z1QtRw5sSor2BJ48CuoKq4Gby5Mn49ttv8dprr6FLly546aWXMGjQIHz77be44447tG4jIYQ4DdULkYemQvQpNVp/QY0eLhQUJxS3tbXhjTfewEMPPYRu3bo5q11OQwnFhBBLapJk9XDyFuLMtnl6QjRxLmdfKCjpv1WtlgoODsaJEyeQkpKito1uQ8ENIYSjdNWJnkd5XNk2mgohfJy9mk5J/60qofi2227Dzp3CJwRCCPEESpNkxarCupsr2+aJCdHEufRWWFBVzs3EiRPxxz/+EcePH8fgwYPtNsqcNGmSJo0jhBBnUpIkq+el0HpuG+kc9La9iKrgZt68eQCAd9991+4+hmHQ3t7uWKsIIcQFlCTJ6u3kbUnPbSOdg95W06neW0roPwpsCCGeRG69EL2dvC3puW2uQHtduZ/ethdxaFdwQgjxdHLrheh5KbSe2+ZMek7w7oyWTc+0W03nrsKCqlZL/fWvf+V/MYZBQEAA0tPTMXr0aPj4+DjcQK3RailCiFp6Xgqt57Y5C+11pU/OWk3n9KXgqampKC8vR0NDAyIiIgAAVVVVCAoKQnBwMC5fvoy0tDRs374dSUlJ6j6Fk1BwQwhxlJ6XQuu5bVrqrJtHdmZOXwr+xhtvYOjQocjLy0NFRQUqKipw5swZDB8+HH/5y19QUlKCuLg4PPnkk6o+ACGE6Jmel0KruF71SLTXFRGjKufmhRdewL///W/06NHDfFt6ejqWLFmCe++9FwUFBXj77bdx7733atZQQgjxNlpWE+5s+SedPYmaiFMV3JSWlqKtrc3u9ra2NpSVlQEAEhIScPXqVcdaRwghXsgZgYhYET9vzD/prEnURB5V01Jjx47FH/7wB2RnX698mZ2djblz52LcuHEAgOPHjyM1NVWbVhJCiBfRupqw3qrDuorcZfyk81E1crNq1Sr87ne/w+DBg2E0dlxltLW14bbbbsOqVasAdOw/tXTpUu1aSghxOT1vEOmpnFFNuLMW8ZO7jJ90PqqCm7i4OGzduhU///wzzpw5AwDo2bMnevbsaX7M2LFjtWkhIcTlOlv+his5IxDp7PknqdEU1BBrDhXxu/HGG3HjjTdq1RZCiE50tvwNRygd3XJGIEL5J4RYkx3cLFy4EK+//jq6dOmChQsXij6Wb88pQohnoE0Y5VE7uuWsQERP1WEJcTfZwU12djZaW1vN/1sIY7OvBCHEs7gqf8PT83kcGd1yRiBC+SeEXCc7uNm+fTvv/yaEeBdn5294Qz6Po6NbzgxEKP/Eu3n6RYGr0MaZhHRytidLZ+dveEM+j1ajWxSIELm84aLAlWQHN1OnTpX9ouvXr1fVGEKI64idLJ2Vv+Et+TydfXUScT1vuChwJdnBTVhYmPl/syyLDRs2ICwsDEOGDAEAHDlyBNXV1YqCIEKI+0idLJ0xbeIt9VhodRJxJW+5KHAl2cHN6tWrzf/7ueeew69//WusXLkSPj4+AID29nbMmzePdtomxAPIPVlqPW3iTSMetDqJuIq3XBS4kqqcm48//hi7d+82BzYA4OPjg4ULF2LkyJF45513NGsgIUR7zjhZykl09KYRD72uTqKEU/X0+rfzposCV1EV3LS1teHnn3+2qkgMAD///DNMJpMmDSOEOI+WJ0uliY7eNuKhl6RgSjhVT+9/O7kXBXoNztyBYVmbndZkWLhwIdauXYs//elPGDasI5HpwIEDePPNN/G73/1O10X8amtrERYWhpqaGppCI53azFUHBU+WShIU1b6O3kY8PJ1Wx7Mz8oS/XU1Dq91FAReAsWB1HZxpRUn/rSq4MZlMWLJkCf7yl7+gtLQUABAfH4/HH38cTz31lNV0ld5QcENIB7GTpdwTYkF5HcYt3Sl4//anx1Dg4gJ0HNTztL8d30WBJwRnWlDSf6ualjIYDHj22Wfx7LPPora2FgAoUCDEw2iRM0KJjvpAx0E9T/vb2U6DunsllV6nwlQX8Wtra8OOHTtw9uxZ3H///QCAixcvIjQ0FMHBwZo1kBDiXI7kjFCioz7QceigpqP19L+du4IzvecpqQpuiouLceedd6KkpATNzc244447EBISgrfeegvNzc1YuXKl1u0khOiQN61+8mTOOA56vSLn40hH6+nfYXcFZ3ovKmhQ86THH38cQ4YMQVVVFQIDA82333PPPdi2bZtmjSOE6N+y6ZkYlR5tdZsnr37yVFodh+qGFsxcdRDjlu7Eg6sPYeySHZi56iBqGlq1bK5DCsrrsD33Mgqv1AMQ72jl8OTvMBec+dhsWu3DMBidEeOU4IybCmu3Sdm1nApzN1UjNz/99BP27t0LPz8/q9tTUlJw4cIFTRpGCPEMeq334mkcHSnR6jjo+Yqcb4RmaEoEDhVV2T1WSc6Jp3+HXV1ewRPylFQFNyaTCe3t7Xa3nz9/HiEhIQ43ihDiefRS78XTaJ274MhxcHdyqhS+wOtIsX1gY0lJR+vM77BY8KqXwFYuT8hTUhXcjB8/Hu+//z4+/PBDAADDMKirq8PLL7+MX/ziF5o2kBBCvJmeRkr0fEUuFHiZJIqZuLujFQteta5P46oLDE/IU1KVc7N06VLs2bMHvXv3RlNTE+6//37zlNRbb72ldRsJIcQrqc1dsM050Yqer8ilAi/bzsyZOSdKiAWvjuYKuZPe85RUjdx069YNR48exVdffYWjR4+irq4Os2fPxowZM6wSjAkhRIgrVuPofcWP0pESZy+/1fMVuVTgNTg5Aocspqj00NFKTfPx0csUoBS95ykpDm7279+Pb7/9Fi0tLRg3bhzefvttZ7SLEOKlXFEfQ+v3cFaQpHSkxBVTWHrd+0sq8NJjRysVvIrRQ1KuHHrNtVO0/cK//vUvTJs2DYGBgTAajaitrcVbb72Fp59+2plt1BRtv0CIe7miVLxW7+GKQExuW129TYDeAgVAmy1DXEnqmInR27YPeqCk/1aUc7N48WLMmTMHNTU1qKqqwp///Ge88cYbDjWWENJ5uKI+hpbv4YqcCKHchafGZ1jl1ciZwtJSanQXjO0Zq6sOlpsK2f70GKx+cCi2Pz0Ga2cP02VgA0jXoHF1fZrORNG0VG5uLr766ivzxphPPfUUXnrpJVy+fBmxsbFOaSAhxHu4YjWOVu/hqmXRtrkLkUFGLP0+D5OX7zU/ZnRGDJ4af4Po67h7VZAr6XUqhI/UNJ8epwC9gaLgpqGhwWooyM/PDwEBAairq6PghhAiyRWrcbR6D1cvi+Y6bG6ayhL3bznJvnpPou5spBJv9Zgr5A0UJxR/9NFHVhtjtrW1Yc2aNYiOvj6sumDBAm1aRwjRBa06TC1X4wi1Sav3cMeyaKnRom8eHQUAvFf6et/IsLMTG23ypJEoT6EooTglJQWMzfyg3QsyDAoKChxumLNQQjEh8jmjw3Q0KVROm7RKPHVF8rOl7bmX8eDqQ4L3r35wKMb2jOW90nd1WwlxNSX9t6LgxhtQcEOIfM7sMNUOxStpk6PD/a5enaN2RZSrV1LJQdNjRGtK+m9VRfz4VFdXIzw8XKuXI4S4mbMTatUMxSttk6PD/a4uVKZ2Sk1P2ybQ9Jh6FBBqR9X2C2+99Ra++uor87/vu+8+REZGIjExEUePHtWscYQQ93H10mM53NUmVy6LVlPWXk/bJnjylgLuUt3QgpmrDmLc0p14cPUhjF2yAzNXHURNQ6u7m+axVAU3K1euRFJSEgBg69at+OGHH7BlyxZMnDgRzzzzjKYNJIS4h546TI4e26Q1NbVcpOqpuGoUwBV1jLwRBYTaUxXclJWVmYObzZs349e//jXGjx+PZ599FocOCSfDEUI8h146TL23yVmUjhb9eUpfhAZaZxqEBvri/6b0dUbzeOlxtE/vKCB0DlXBTUREBM6dOwcA2LJlC26//XYAAMuyaG9v1651hBC30uPOv3pskx68sPEEahvbrG6rbWzD8xtPuKwNnWFkTWsUEDqHqoTiqVOn4v7770dGRgYqKiowceJEAEB2djbS09M1bSAhxH30uPOvHtvkbq6qpixFz7uK65WrAsLOlqysKrh57733kJKSgnPnzuHtt982F/UrLS3FvHnzNG0gIcT99FhkTI9tchc9rZbS667ieuXsgLCzrl6jOjeEEN3xhqtMV34GPda5oZE1+ZxZT8mbiju6pM7Np59+ir///e8oKCjAvn37kJycjPfffx+pqamYPHmy2pclhHRi3nCV6Y7PoMfpIBpZk89ZU616ma50B1UJxStWrMDChQsxceJEVFdXm5OIw8PD8f7772vZPkJIJ+INS2Ld9Rko0drzaV1PqTMnK6sKbpYtW4Z//OMfeP755+Hj42O+fciQITh+/Ljs19m1axfuvvtuJCQkgGEYbNy4UfI5O3bswKBBg+Dv74/09HSsWbNGxScghOiNnCWxBeV12J57WbfLY925rFdNfRy90Ptx9VSdefWaqmmpwsJCZGbaXw34+/ujvl7+l7O+vh4DBgzAQw89hKlTp8p637vuuguPPPIIPv/8c2zbtg0PP/ww4uPjMWHCBEWfgRCiL1JXmY99kYUTF2vN/9bjdJUeEns9aTrIG6Yh9UyP05WuomrkJjU1FTk5OXa3b9myBb169ZL9OhMnTsSf//xn3HPPPbIev3LlSqSmpmLp0qXo1asXHn30UfzqV7/Ce++9J/s9CSH6JHWVecoisAH0OV3Vma+U1fCGaUi966zTlapGbhYuXIj58+ejqakJLMvi4MGDWLduHRYvXoyPPvpI6zaa7du3z1wwkDNhwgQ88cQTgs9pbm5Gc3Oz+d+1tbWCjyWEuI/QVaaBAUwsYLJ5vB6TIjvzlbJSnTnZ1ZU6a10oVSM3Dz/8MN566y288MILaGhowP33348VK1bgL3/5C37zm99o3UazsrIydO3a1eq2rl27ora2Fo2NjbzPWbx4McLCwsz/cdtGEEK0oWW+BN9VZu8E8SWfekuK1MuVst7zWDpbsqu7j4crN3/VA9VLwWfMmIEZM2agoaEBdXV1iI2N1bJdmlm0aBEWLlxo/ndtbS0FOIRowBn5EnxXmSzLitZwsZ3qEasv44raM664Uhb7HJ6Sx9JZpvA85Xh4G1XBzbhx47B+/XqEh4cjKCgIQUEdX9La2lpMmTIFP/74o6aN5MTFxeHSpUtWt126dAmhoaEIDAzkfY6/vz/8/f2d0h5COjOxfAlHi4PZJsXKmeoR60RYsKo6GEeCIWck9srpKPmOy+78cjy89hC+fmSkpu1xRGeZwnPm74QIUzUttWPHDrS0tNjd3tTUhJ9++snhRgkZMWIEtm3bZnXb1q1bMWLECKe9JyHEnquXPMuZ6hHrRJQmrlY3tGDmqoMYt3QnHlx9CGOX7MAvl/2EY+ertflAKkl9DqHjYmKBQ0VVuG/FXtQ0tLqsvVL0MoXnLLTjt/soGrk5duyY+X+fOnUKZWVl5n+3t7djy5YtSExMlP16dXV1yM/PN/+7sLAQOTk5iIyMRPfu3bFo0SJcuHABa9euBQA88sgj+Nvf/oZnn30WDz30EH788Uf885//xHfffafkYxBCHOTqJc/cVM+uM+XIPleFQd0jcEtGjPl+qeRUPmKJq3xBxIkLtZj0tz1um1KQk4ArdVyOFFfpasTA25Nd9VAaoLNSFNwMHDgQDMOAYRiMGzfO7v7AwEAsW7ZM9usdPnwYY8eONf+by42ZNWsW1qxZg9LSUpSUlJjvT01NxXfffYcnn3wSf/nLX9CtWzd89NFHVOOGEBdzdb6E1HSMVCcixraDEQoiOLvzylUHCI5Mc8npKKWOiwlwaCWSs3KWPKk2jxKdJa9IjxQFN4WFhWBZFmlpaTh48CBiYq5fOfn5+SE2NtaqYrGUMWPGQGzfTr7qw2PGjEF2NtVAIMSdxJZtD06O0LzjlMpbUDW/fo1tByMVRHABwpcHSzA8LUrWZ9UiqVROR5ka3QWjM2KwO78cJpEtkZWOGFBSrDqdJa9IjxSdE5KTk5GSkgKTyYQhQ4YgOTnZ/F98fLyiwIYQ4tn48iW43I5frdiL745elJVTwJffMnPVQXNuiJy8BdsaOLb6JoTCh2GsbvNhGIzOiLHrYKSCCM4f1x+3a6sQLYrVcR2l1OdYNj0Tg5MjRF9L6YgBFdtTz9vzivRK9QXPp59+ilGjRiEhIQHFxcUAgPfeew+bNm3SrHGEEP3i8iWGJkfYnUgOF1dh/rpsWZ2/VMepxXTMG/f0k93BcEGEgbG7i5dUJ69lUqmcjjIsyIivHxnJe1yEAjoxlBTrGE/e88uTObQr+C9+8QurXcEjIiJoV3BCOpGC8jocKq4SHTkR6/zldJxypmOkRjX6J4Ur6mCWTc/EzekxvPfZkurktSxWp6Sj/GjWUNycYf0Z1IwYdLZie3IpLcrX2YrouZuqOjfcruBTpkzBm2++ab59yJAhePrppzVrHCFE3+Qk8oqtSpLTcY7tGSsrb2HZ9Ew8ti7bKi/EtjOXm7jKBRF78svxh0+PoK65XfI5QnksUsHZB9vzMSgpQtGVvJzPodVKJEqKtUb5R55B1ciNVruCE0I8m9z8FID/Cl9uxyl3Okbr4f+/7yxEY4tURo91W20JjSpxsoqrnZq74uiIgdxcH1tabzfg7u0LOJR/5BlUjdxwu4InJydb3a50V3BCiGcTWg3Ch6/zl7uaRMkohFbLiqWWhAu1lc+y6ZmY/ckhHC6usrvPdmTLFVtEKCVnVIyj9ciGnkZKaLNPz+FRu4ITQvSHr+OzJNX5K+k4nVEPhS+YqG5owYIv5V2Jc20VC0rCgoyYPy4dD64+JPg6Jy/U4OVNJ3k78Yr6ZhRXNsCHAdpZuDzwURJcar3dgJ62L6CifNfpMQi3pCq4efjhhxEYGGi1K3hCQoLTdwUnhOiPZcd38mINPtlbhENF10copJJY1eaGCJ1c5Z50xUYEFqzLwamLtaLv/+bUfhieFoWIIKNdcMY3siA1BffJ3iJklVRb3bYnvxxjlmxHFc9qM3eMXkgFl1qPbOhtpITyj/Q1kiaGYcWq6Mmg913BbdXW1iIsLAw1NTUIDQ11d3MI8UrOLKcvdHL985Q+eGEj/8gH30l35qqDvNNhg5LDrYIzWwYAN2fEmEcNhF5nVHq03ciC2vfkI/Qe7rQ997Lo6NTqB4dibE/5fYXWr6cFJcfbG7nz8yvpvx0p7InLly/jyJEjyM3NRXm59Nw0IaRzcOayV6FpisnL98hO9BRbgi4VZPROCDWPRCmtASOUGD1rZIroe/LRY50ZrUc29DhS0pmL8nlSzSNV01JXr17FvHnzsG7dOphMHSsJfHx8MG3aNCxfvhxhYWGaNpIQQgDxaQq+qRuh6QtH9qIKNF6vxK40B0NoCq6gvE51e/SU56H1dgNavJ7WuSHevtmnGE/KOVI1cvPwww/jwIED+O6771BdXY3q6mps3rwZhw8fxh/+8Aet20gIIQDUByW2y9CVLGG3dai4Co98dkTW6wiNLNiObEktF1fzHu6i9ciG2OuJLQ+X2tZDLe49AXS6onx6HEkTomrkZvPmzfjf//6Hm2++2XzbhAkT8I9//AN33nmnZo0jhBBLaoMS25OukiXsfPYVVODouWos/f4M7/1yRxYsRxX4Vo1FBBlR09DKWwFar5svaj2ywfd6cpK4HVllJbiCzgMSaZ3JkzYCVRXcREVF8U49hYWFISJCfMM2QggB1E0XiJ1cQwN9UdvYJvukK7WEXcrTXx9FQTl/joHUSIVYR1nZ0GLuxCOD/ATbqIc8D7FjqPWyfcvX45JaLVkGLmpXWUmtoNPLknQ1tJqeU1K6wZ1UrZb68MMP8fXXX+PTTz9FXFwcAKCsrAyzZs3C1KlTdT01RaulCHEvR6+Aaxpaea/a/29KXzy/8YTi1y28Uo/9BRVYtP64yk9kb/vTY0Q7EDkrTiw7I6Bjas3XwKDNxLo9z8OdoxgF5XUYt3Sn4P3bnx6Doop6Vaus7lu5F0eKq2Cy6BV9GAaZ3cN5CzBavqeeRi0sOetYuSPnSEn/LXvkJjMzE4zFfHBeXh66d++O7t27AwBKSkrg7++P8vJyXQc3hBD3cvQKWGzaQ810CDci8O3Ri9h7tkLdh7IhllgpNapw9FwVln6fp+vpD3eOYmixS7ztNGV1Qwse/uSwYAVpscCGe0+9BjfOOlbOKKipJdnBzZQpU5zYDEJIZ6BlUTahk6vak65jFb+siSVWSnXOz284gdOlV61uk9MZuapirLsL68kJXFKjuyjKDVmwLgdZEgGM1HvqkbuPlTvJDm5efvllZ7aDENIJ6HUpaUF5HfYVKBu1GZoSgaziasWJlVKd8wmeyshCnVFBeR1OltZirU1VaGeO9Lj7GMpNan12Qk9MOXsFsAhaGQb44509rV5P7h5iao+3O7n7WLmTQ0X8CCFECb0uJVWyxNyAjuDho5lDVS15TosJRoRA0BHs78N7O4db0m65zPmxL7LtCg86c5dqPRxDOcvNf/fxAbSZrIfj2kwsZqw6YHWb1LE3MMLHu1d8CJ4ef4Oaj+ASejhW7qJqtVR7ezvee+89/POf/0RJSQlaWlqs7q+srNSkcYQQ76LXpaRKlphzFYod2ROLr+AgANQ1t4s+l+uM+PIoLDlz2kEPx1Dqb78z97Lg37iqoRU/5ZXjlowYANLHfnByhNXxPnquGs9vPI4TF2px4mItJi3fo7ucKI4ejpW7qBq5efXVV/Huu+9i2rRpqKmpwcKFCzF16lQYDAa88sorGjeREOKp+Iqs6bF8vZIiesvuH2TViSndakJNIUIfhsHojBhzNWO+Evh8bIsXakUvx1Dob59zvlr0eVkl10e6hI69AcDQ5Ah8/chIq+O99PszOH2RPydKj/RyrFxN1cjN559/jn/84x+466678Morr2D69Ono0aMH+vfvj/3792PBggVat5MQojNiCax8y0/7JoTijXv6oX9SuC7L1zta90YuNYUILTsjJcGRs6Yd1IxauSrhGQAGdgsXvX9Qd+t6bHzH/uZrozGWPDFBt7NuF6EquCkrK0O/fv0AAMHBwaipqQEA/PKXv8SLL76oXesIIbojp24G37SJ7RC+3paScp3AuoMlojVvHE3CVFIdefHUfrgpLcrq/eQER66aduCOITdClxLVBSzLWgUx7qiJc2vPWEQEGXmnpiKCjOYpKU5FfTMevDkFc0anitYR8uQEXb393pxNVXDTrVs3lJaWonv37ujRowe+//57DBo0CIcOHYK/v7/WbSSE6IhU3Qyp1Se788t1XdU1ISxA9H4tRkPkjhLFhQXYdUhygiNXTTvwBS6WRmfEoM1kwoEC6zxMV9TE+Wb+zZi0fLdVgBMRZMQ3869vGyQWePHpzAm6nkZVcHPPPfdg27ZtGD58OB577DH89re/xapVq1BSUoInn3xS6zYSQnRCzrC81NWtiYXiIXxXTGlIddRq9osSeiw3SrTrTDlmfnxQ8LWEOku+4GhocgQeGJmC3olhLrtCl0ps3p1XzrsvliumcZKigpD90nj8lFeOrJIqDOoeYTdio7TAXWdN0HXllKJWVAU3b775pvl/T5s2Dd27d8e+ffuQkZGBu+++W7PGEUL0RYvqsJaPlTpR5pRU4YVNJ3DiwvXaL1pPaXAn7g+25yOruFrwcY7sFyXU1tE3xKjqLB1ZqaVVJyWnPgxfYGNJznfA0TbfkhFjF9Rwr6smf8ZT9lbSgidvFqoquLE1YsQIjBgxQouXIsSjeeIVjhJKqsMKXbVbPlaI2CiKI1MalscnIsgoOlJj6dPZw3g7SEtqy9zL6SyFvldy8yic0UmpWfVlS+l3wDIp3VFq82c6U4KuJ28WKju4+eabbzBx4kQYjUZ88803oo+dNGmSww3zRN7esRFhnnyFo4TcYXmxnBI5Q/gL1uVgt0DQoWZKg+/4BPv7oF6irgzHthicLbWjAFyF4cbWNtntVvO90rqTqm5owQfb82U/3sDAbjNKOd8BqaR0R35bjubPeHuCrieuDLOkaG+psrIyxMbGiu4zxTAM2tvlnTC8RWfp2IgwT77CUUrOSAN3dXvsfDX+tOG41bSS1BC+3HL4Slam8B0fqYJ5lqQ6OqWjAFL5PXvyr2D2J4fQ1NaOUzbbMSj9Xjmjk+rYi6la8nE+DINhqZEw+hgUTeO4IindmfkzWl3ouvOC2ZNXhgEKghuTycT7v0nn6tiIPU+/wlFKybB8/27h2PzYLYqG8OVOd8hdmSI3WBISEWSUbLNUNVRfg3WBODkVhoV2olb6vdK6k1Ly9+SCmLAgo+zvQEfgJ14QT01SOh+t82e0utDVwwWzp68MU5xzYzKZsGbNGqxfvx5FRUVgGAZpaWm499578bvf/Q6MjAqf3qSzdWzEnidc4TjjClDJsLySx0qdVA0McHN6jFMrAluqamiV/B1LXe5ZTms5Gmxxiirq7WrKWOKOuVTVZaWdlNTf882p/dA1LEB1ftCCdTl2o1VCHP1taZ0/wxe07s4rx4yP9mPZ/YMcGml09QWzp68MUxTcsCyLSZMm4T//+Q8GDBiAfv36gWVZnD59Gg888ADWr1+PjRs3Oqmp+uQJHRtxLj1f4ejhClApqTouN6cL1yHho6YisC3udywUJCr5DmiRiAsAH/yYj0PF9juBs2DtjnlEkBG1ja1ot8l7GZQcbt6iQavgc7hN0UEllAZ+Wv22tMifEWq7CR25QmOX7JD129PTBbMnrwxTFNysWbMGu3btwrZt2zB27Fir+3788UdMmTIFa9euxcyZMzVtpJ7puWMjrqHnKxw9XAGqwXdSVbNSprqhBa98c8rh9kQGGTFz1UHBIFHJd8DRYMvAAMH+vsgqqba63XJ/I9tjXt3QipAAX9Q2XU9cDg30xaGiKjy4+pDV56mobxYd5ZMKPp/5+igeVFlvR27gZ2A6NjDVEzltl/Pb09MFsyevDGNYVsbua9eMHz8e48aNwx//+Efe+9944w3s3LkT//vf/zRroNZqa2sRFhaGmpoahIZq8+OYueqg4ElNzx0I0U5NQ6tdZ+zuEZKC8jqMW7pT8P7tT4/R/YnK0ZMq329TqRFpUTD6GCR/43K+A1KJxM7WLzEUf7i1Bz7ZU4Sskmqrz2NAR2dmWdFX6DvM91n5KP0NSH1ntXgPZ1HSdrHfnrN/t9enK4F2Fh4VsCjpvxUFN3FxcdiyZQsGDhzIe392djYmTpyIsrIyRQ12JWcEN3rs2Ih76OkKZ3vuZfNVOZ/VDw7F2J6xvPe5u6yB7fsraY9lrolY9V+5MpPCkX2uWvB+285G7DugRbDlCAMDDE6OwKEi/mRlW2IXaXI6czUXeZmvfc+7J5SW7+Esco+v2G8PEP4bRAQZkf3SeKvb5P42xAJrT+mvlPTfiqalKisr0bVrV8H7u3btiqoqeT8ab+LJQ3dEW3qqfaFmytTdOTp872+7AaJQe5SOiiRHBeJcZSMkStiIBjaA/TSB0HdAq0RiR5hYyA5sAPE8DznTMErzRArK6xQFNmrew5nk7hkmlq4g9jewTG5X+lsVW6HnCVPVSkmtYLTS3t4OX1/heMjHxwdtbfzFqDqD1OguGNsz1u0/MEKA67kRtqtlfBgGozP4VxuJ5ei4At/7257ohdoz7/MsRcHDst8Mws3p4lWH5ZCTV1dQXodvj110+L3EcMd1dEaMshO7DFzSsSUluUN8z+fjSLK13PdwJu5Cd/vTY9A3MRQ2FQBEf3scOTk3gLLfKhdYC40oWQaI3kLxaqkHHnhAcOfv5uZmTRpFCNGGktUO7l6lIXdkg689BeV12Hu2QtH7hQReH3H99ugFvLs1T/Cx/RJCcar0Km/n8PKmk4JXy0pGkz6dPQznqxqxaP1xRZ+DY3lcZ39ySLBODtCxyaZtzo0Y2wCOmwqR+zpyF1Y4kmztyOINradhU6O74PPZN6laaSRnxFXpb1Vu0Gg7Cunu6WlHKApuZs2aJfmYzrRSihC9kztlKqdwmrNXaSi9ardsz4FCZYENABwoqDBPIfXrFi762Edu7YGvDp9XvNeVVLE+4HrOCLd31X+Pl8nK2+CWcs8bm253XP81dyTuWb4b2edq7J43Ii0KK3872K7j5VsuzuXocK8tZ9qQ77PJ/d5IrcTS4j0sOXMaVui3V1Beh6xzVYpXo1l+zu25l0Xf2/a3Kjdo5AJEd09Pa0FRcLN69WpntYMQ4kRSuUByCqc5u6yB0qv2D37Mx6CkCIQFGXGmrE7x+1l2nVLvHRLoi4n94hRdLcsdibK9mucbbRuRFgWGgdXolGX1Xz6Bfvynd4bh73gjg/zs3pfL0Zm56iCWTc/kDdZqG9swNCUCvxrcDf/33Wm75eb/N6Wv+e8hZxRALG9lZI8osCywr8D+76CGK0olcL+96oYW0XIClqRGXJXm00kFjbYBoqeWkLCkya7ghBDPJdUJC1UE1nrIWulVe1ZJNR5bl42/Th+I/51UvkLzprQoyff2YYDQQCNmfiy86oxje7UsNRL15B0ZmDQg0e5vJzbaJnfRgtg03d6zFeZAzDboXTt7GO75225kn7ce8dmVV45ZHx9Aznn7kaB2lu1IUmZhtxFpbWMbnv33Mbu9pcRGAWw/v6+BQZuJVfV3ECN3aker77mSgEFqxFVNbS2xoNEycHL39LRWKLghpJOT6oR7J4RaXRk7c8h62fRMPLz2kKwVPdzJds7awyitaVL0PiN4qujynfxDA42okbl6x4fpWH7PdURSV9d8gY0lvtE2uavx1BaCKyivswtsOHyBjaVDPDk+7SxrNcrC2ZVXjrmfH8EXc24yv69tACH2WbVYlSj1NzpxsQYvbzqpyfdcbcAg9jmVVg+WEzQC+ioi6AgKbgjp5KQ64WXTB1mdzOd9nmU3KmDbWakVFmTEvLHpovV5bEkFQgYGVsu9uQ6K770tT/4+DGSN2PCN7nDv4a7K1Worpx8orHRGc3jtPVuBo+eqsPT7PIcDCDWjK1J/o7V7i+x2Plc7NaNlwGD5WdWUIJEKDL2l6j4FN4R0ckqGuOVOd8ihdp8mpbjApm/ite0bJJKHuZO/VNImh290h+sE3bE3j3klU0oEsoqrFQZW4tOBGbHBKCivt3vNQcnhiurncJ7++hgKyq2XHysJIBwZRRT73md25/88aqdmtAgYxD6rloGynreTUULrcgiEEA0UlNdhe+5ll9WdWDY9E6PSo61u4+uEpVYlHeCZgrDFJVaOW7oTD64+hLFLduCXy37CsfPVAITr89gyoKOzlev0xatY8r8zgvfb/s2lOqQ3p/bD2oeGoqqh1W5XcK4TrGxoMdc9Wf3gUGx/egzWzh7mlBUntn/XQ0VVCA20vn6VCqyGp0YJ3gcAS+8bwPs9+WjmUFnHzFbe5Tq7/ColNVccrcsk9L1/cGSK6POU1tRRU3PKlitrUMk5H4ido0ysCTVN4tOYzkYjN4ToiLuWYMqvsi3eeclZvMt3kj5xoRaT/rbH/FnlVHo1oaNzlEvoqlvsby52BfubYd1lL8l1ReXqhz85jCybvJfaxjYMTY7AvHH2y8X5pMUEY2SPKN7RuZE9otA/KVzweyK3Oq9cUlM1WiS+ii3XFqNmasaRUTxXJ/mKnQ9sfy8sWtEv+SomDmpGbuUxZJdl42jZUdyWdhs2TNugWZuUouCGeC1PLEDl6BJMRz+zVCc8PDVS9PmWK5CE2ifW+e3OKzevgGpsdU61c9tOU+xvrvWSXGeobmjBnLWHeYv2tbMsDhUL11Ths2KGfQ0c2zwlvu+JZYe4v6BCshhhZlIYbx0ejtTfzpGkaankZaGpGdu6P0qwskJ/fu5K8rX9u9Q212LamnU4cjEbTcazaDGcRStzDiWX2/DdFuvnnio/pXl7lKDghngdTy1A5cjVmdafWShISosJxoi0KN4VMIB4tV5A+iRtAswroI6IVNgVwkB69Iir8Mptrin2N+emlbRckqulgvI6LPgyGyclahQ9ti4Ln8++SdZ3oaK+GQ/enII5o1N5V9NISY3uIjlt0zcxFGseHI7H1mWr/tspDSyV/kb4Alvbuj9KfluOXLg4I4gWuxBiWRaldaXILs1GTlkOsss6/v/ZqrMdD7D52Aa2C4ymHvjtoFtxa+pQZMZn4sboGxW3SUuKdgX3Bs7YFZzoC9/OvHraOViII7t4a/WZ5XQANQ2tgtMPUu8pZydpZzEwwIBu4QgJMMqeOpHavRng/3s4O5hWukko0DGtJLaaTcsAWeo4czupO/q3+9WKvcgqrrLKeRL6Dqr9jdy3Yi+OyHwPIXL/HmKU7BQuxn5ayYTMlGZMHtaM3Mrj5mDmcj3/lKuPKRp+bBr8TD3gZ0qDH9sDPmwMGDCyfi+OcNqu4ITonScXoFJ7dablZ5ZzdRkWZMQrk3rznqyl3jMtJli0XL8zmVjpHb5tybkilp+vpB2+/BopUqvZtKxKKzWixbKsuSaQmr8d10HzTcXx5bGo/Y0UlNcJ1u9R8ttydFpJ7k7hUpramjDjk69w8EIWmoxn0coUoMVQiJJLTdj0rfVjDYwBN0bfiIFxA5EZl4nMuEyE+fbArz44Kfj6H2y/XjXc3Si4IV7FkwtQqZ3i0OozK+kA1L7nztzLDgc2oQG+qG9us9sDyaThGLTU31xp0TmtiOXXyMHtp2XLGdV6+aZ1hqdForXdZBUYq1nOzBeIcfkwfIGY2u+rVr8tR6eV1LSjqrEKRy8d7ZhaupSD7NJsnL5yGm2mNrtpJYb1h5FNwb39RuHWlGHIjM9E39i+CDLat3t0xmXBKuJZxdW62aKBghviVfSQ4OkINSsqtPrMSk6gWuQ7iMlMCucdZcnsHo41Dwyz+xvdnB6D2sZWHDtfbbcsWw2hv7lWUzdqE78XrMtRlYvEEYr/nFGtlxvR2nXmMrLPVWNQ9wj8Y1ehw6NDQoEYlw/DN4qh9jei1W/L0dwssXawYOHvX41vcw8guyzbnB9TVF3E+3gDG9oxnWRKM08v+bIJYOCDOf34p5Usv6/LpmcK7jqvpxFyCm6IV3F3gqej1ExxaPWZpU7kl2qazCctpe/JV9WYD7ePVWs7f4gS4Otj9zeKDPLD0u/PIOdanRxHCF39cyf3D7bnC1atfWVSb8mAxZHgSO5GnGK6RQTy3n5ZYvsKNdV65Qa0rpjiUfsb0fJ84shScK4du/MvoQnn0WooQAtTgFZDAUzGIoz6pJr3eSnhKciMy8TAuIEYGDcQkcYbMOPDfDACJR3kJmE/ODJFdPRQDyPkFNwQr+OOqrBaUzrFocVnltq48o/XlvZynfGfp/TB5OV7rKaZLHeB5ohVNbY1ODkCT43PwOTle3nv31dgv+kjlyhqycAAfRJCcfyC+CoiW7ZX/3I6aK5z5ptqsQ1YHMlrkerU5WizmbsrrqjHFJtjaMmRar18n1WMVlM8lrvFW1L7GxF63lPjM6z2EpOi9MKlsbURxy8fN69YOt2ehZLAo2hnm60faAJ8GB/0iullzo3JjM/EgK4DEBEYYfe6t2bUyg7WhL6vDS3iZRr0MEJOwQ3xOu5I8HQ3rT6znEJsllVRaxutT3K1jW14fuMJq45aqqqxpXlj01EpkZNj2QmKTVEcv1CLockRyCqp5g3W5LyH0g6asyf/Ch5eewjzxl4vnudo4rcW21LYdjpigQ3Q0YlPG9JN9Cr9gdUHsGz6IKttLdSMMjk6xcPhdotXutO2EPuRQiOWfp9nFYArmZrku3CpaKiwWnKdXZaNn6/8DBNrP4IZ5NsFqeG9kRk3ELemDsPAuIHoG9sXAb4Bku8NyA/yxL6vh4urVG7v4ToU3BCv5YoET71x9DPLKcTGdcZ8+Dtq+SX5I4OMeH3zadHHWHaCUqMZE/vFobGtHScsRnBGZ8Rg+rAkzP08S/Q9HJkGamdZHCqqMi/tH50Rg18P7Sb6HKmRC7GVZkF+PmhoaRd8rgHAzTZl/qWSu9/5VX/cNyRJslpvcUWjVXXpsCCj4lGmiCCj4pFKod3jHdlpW4zYSKHc0TeWZVFSU2IVxGSXZuNc7Tnex8cExSAzPtM8IjMwbiDSI9PhY/BR3H6O3CBP6hjOGpmCQON53Y6QU3BDCLETEWTE5/uLVT/fsqOWqmps6YHVh+w2oeTwXRVKjWa8ZhEo9U24tnFmUjgASOZSyN04U449+VckKy6LjVwUlNfhQGGlYDAiFtgAQHCA/XShVI7SxZpGAPKX73PVpdfOHqZ4lElsOTNf8rWc3eOdkfehZPStzdSGn6/8jOzS60m+OWU5qGriHwXrEdHj+rLr+I5AJj44HozC/brkkgrypI5hn4QwrJ2doNsRcgpuCCF2FqzLwSmJqrdiuI6a65ikSu1zxDrQQcnhdleFUlMUlk6XXsWS789g7exhKCivw6+HdkNja5vV1b+SrRWU4EZy+KbJDOAv6V9QXoeTpbVYu7dI1o7bPgzAsuBdLVbf3G43XThQYnf0Qd0jzO2Qs3yfqy5deKVe0XHh2AYjUsnX7lgZKTSaYUITWg2F+Mv+k2hg85Fdlo3jl46jub3Z7rG+Bl/0ienTEcB0HWjOjwkLCNO8vY6Qm0yt1xFyCm4IIVakpmO4FU0ABE98EUFGzFx10Op1HC3eN29suuwy+Xy4q+v7Vu61ChaGJkfggZEp6J0YZre1glCbA40GNLYqX3T+wMgUBPpZD+WbABwqvl7SnwWruPpwx+cDfAzgjW74RhZu7Rkr+Pkigoy4JaPjGCudYuKCFKUbadoGI1LJ1+5YGZkcGYR21KDFcBYt11YstRgK0MZcABgWfzli/fhgv2DzaAz3/3vH9Ia/r7/mbXMGJUnYetvLTxfBzfLly/HOO++grKwMAwYMwLJlyzBsGP/c5Zo1a/Dggw9a3ebv74+mJvGljIQQeaQ6s94JoVg2PRNFFfWorG/GCYsRHu7Ex+0ZZKmmsRX9EkPxh9E98JdteYp29AaEr8SVbkhoWycmq6QagX7nsXZAgtXtYiMWagIbAKhvaceDN6egqqEZJy/WWhUetEzUVpPEDAACK+jNbEdHvpl/MyYt3231OSOCjPhm/s3mfysdweKOk21uhw8DzPxYeBrJktzpH2eujGRZFoXVhXb7K10IvMD7+EBDNMakDbVaet0jsgcMjMHhtriLnPwcve7l5/bg5quvvsLChQuxcuVKDB8+HO+//z4mTJiA3NxcxMby71ERGhqK3Nxc87+dNSdJSGck1Zn935R+dh1K38Rr+SzdwiVXMP1pw3FcbZK/4ze3HJnbjNFypVRxZQP++sMZWVNelu2wJJSAqmbptYEBgv19Ud/czjsd8+y/jwk+VyxRWyu2AWJSVBCyXxqPn/LKkVVShUHdI8wjNhy5U0xCIybctIVUDpNl4CW3lo1WqwRb21txqvzU9STfa4FMbTP/1GyIbxLam1KuFcFLw+iUofjot7frYtsBZxCbetJy2w4tuT24effddzFnzhzzaMzKlSvx3Xff4eOPP8Yf//hH3ucwDIO4uDhZr9/c3Izm5uvznrW16vMICOkMpIb7l35/xu5kdvriVSz5X0c+i1THVKsgsAE6auccLr6+8mhkjyiwLAR3JlfLdlRDTc7Nzekx+L8pffH8xhNOD1SE9EsMxamLVxVN1dySEWMX1FjiGyEJ9vdBXfP1RGYtK2krzadRkvdxtfkqjl462hHIXEv2PVl+Ei3tLXaP9fPxQ9/Yvlarlfp37Y8Q/xDRgEpvUzTOoue9/Nwa3LS0tODIkSNYtGiR+TaDwYDbb78d+/btE3xeXV0dkpOTYTKZMGjQILzxxhvo06cP72MXL16MV199VfO2E+LNhAuX3YDJy/fYPd7yZKZFIm5GbDAevz0Dn/BUxpVbEFAp2w5TaVLsp7OHmQOEjm0HyjHz44NOaauQiCAjPpt9k92x6xUfgqfH36D6dYVGSJxVSVurfJqyujKrICanLAf5lfm8U5lh/mEYEDfAqhBer+heMPrwj8bwBVR6naJxFj3v5cewrMLqVhq6ePEiEhMTsXfvXowYMcJ8+7PPPoudO3fiwIEDds/Zt28f8vLy0L9/f9TU1GDJkiXYtWsXTp48iW7d7OtI8I3cJCUlydoynZDOzrbz2p57WXT5bd/EUHx+rXPdnV+uejNLbhsEOauEHMUlSPMNodc0tMpOil39YMe+PNxV+6WaJnNVZzm4jvv4hWpZidc+DKw2D+VyZZKiOoLLo+eq8fzG43Y1ftzZ0fL9PYXapOSxJtaEs5Vn7aaVyurKeNuRGJJotVopMy4TKeEpDqc4cDVw+AIysSkaTx3pKSivs6rMbWv702M0/Ty1tbUICwuT1X+7fVpKqREjRlgFQiNHjkSvXr3w97//Ha+//rrd4/39/eHv7xmZ6YToje3VqdSozKmLtXhsXbZokTU5uG0QXCHIz8euBgyHG7G4b8VeHC6uEk1djgzys1shpoTYyBjn1bt74esjF3DiYq05sEmODMSiib1wZ794q8cu/f4MTl+8anWbUC6EqzpXJTkyQo9tbmtGVmmWeUQm51IOjpYdxdWWq3avwYBBz+ieVquVBsYNREwX4Sk4tdRM0Xj6SI+e9/Jza3ATHR0NHx8fXLp0yer2S5cuyc6pMRqNyMzMRH5+vjOaSAixYN7AL6+ct56Kie2odVLZ0IKvHxmJ+1bsxZHiKqvH+jBAaKBjy8K11NBiXwPGUkF5HQ6JbD/Ajfzw5SJJCQ3wxV+mZyIlqgtKKuqxem+h6OM/O3AOZ21WmZ2vasIXB89ZBTdyO1pXdK58gZPcHJmaphqcqz+KY1XZ+OR0x2jMyfKTaDPZ5235+/ijf9f+VkFM/6790cXPNR2smikaqWRcPY/ocG17ekLHdKfeKhW7Nbjx8/PD4MGDsW3bNkyZMgUAYDKZsG3bNjz66KOyXqO9vR3Hjx/HL37xCye2lBDCWTY9EzM+2m+1BNwWdyJ/4Ze98fyG4zbLxTs6z8qGFsEtHjhq94ZSggvIhJIf5SyNlxpxEVLb1AajAZj6gfgeTxy+5fN8IwNyO1pnrnRREjixLIuLVy/aTSsVVBXwvnZEQAQy4zORGtobsYG9MDplCG7PGARfg/O7NKGAQ2kStFQAaluPSS8jOkLH9Zv5o1DR0KKbQMzt01ILFy7ErFmzMGTIEAwbNgzvv/8+6uvrzaunZs6cicTERCxevBgA8Nprr+Gmm25Ceno6qqur8c4776C4uBgPP/ywOz8GIV5BzpViWJARz0zoiVkiuTdNLe12UzSWy8WBjvo0/z3OnxPBDWvzJTYPS4lE7qVa1DTKW3U1NDkCL/6yNx5bl4XiykbBxwklP0p1WsumDzIvU1fjkc+yrFYdqcW1v7qhBR9sFx/JFts7S6uVLkKB0/wvDuPFKZHILsvG9oIDyCrNQWHNCVQ08o96JYUm2e2vFGqMw+NfHsUP+zva/wXKMTojy6mdv1SwpnSKRioAta3HpPXyarWjQkLHFYBbl37bcntwM23aNJSXl+Oll15CWVkZBg4ciC1btqBr164AgJKSEhgM14sgVVVVYc6cOSgrK0NERAQGDx6MvXv3onfv3u76CIR4PKXTE1Il7JbvyLfL97BcLg7wnyQ5XGDDl3fx8qaTqGuSHwwYfQ3onxSO1yb3FQ3IfA38yaRCnZbltgmOrMvQIrABgA+252NQUgTmfZ4lmK+kZO8sOStduA7Sh2HQzrJ2O6CzaEELU2yu6NvKFOCL84X4fLn9tgQMfHBjdE8Mir++WmlA1wGICoqyeyzf5pW788vx8NpD+PqRkaJtVor7jB9sz7dbuWcbcCgpKigVNMutx6SUI1ORel76bcutq6XcQUm2NSGeRu3VmNJVHlKrJMRsf3oMWJZVtcpC7ftuf3oMiirqRVd6LZ7aD3FhAbx/O7FVU1zHMPfzI4qWqfswDLpFBqC4Qng0SQmu2OFhkfygockR+GjWUIQFGVWtdOG+X5FBflj6/Rmrv0c76tBqKEC32EsIDT2P3cWH0cqcAxj7UNgHAfA1pcBoSoWfqQf8TGkIQApuSU+UvPqXarflZ3QEXxAgxPZvJXeJPN/vTmAHDTNuVZ4cfOcDtSu6AEiullTSNjW8erUUIcSeK6/GuBMmXz6MD8OgV3yIaD7OY+uy8MitPUTbtL+ggrdzUFM1GOgYhZC6UrbM/eH+dhX1zebOYe3sYbhv5bUEaZ5tE5ReJo5Kj8ZvhnXDvM+zlT1RQDvLigY2AHDv4G7mwEbsGNpOo1h+v1iwaGfK0cIUosX3+h5L7YaOkaBLNQBq0NFLAzCwodcCmFT4sR2BjC+bAAY+Vm1jIZ77xJEznaPF9I3YyKIt21EuuQnTfCM9g5IjRI+jnA1Bhc4HT43PcGjkxR2blapFwQ0hXmDuZ1l2FXt35ZXjkc+OYN3vbxJ9rtzkU74Tpu3Gi3KWM5+6WItP9haJvidfoCFnJ2ghXJAktyjfnvxyjFmy3eqzZcQGiyb0ijEwHYnHz915I9pMrFXQFhF0gjeZODTAV3E1ZykNLW2SG5paTqO0mdpwpuIMfv/lv3Dicg6a/c6ixVAIE2O/7BoAfE1d4cf2gNGUinDfG9DWlAwfRIHB9Sk/28rGtqSmwySncyAvSBIjtXmsLbWdutByd7HRFTmfiS8w251XjuJK8f3cpP72el76bYuCG0I8XEF5neBWBPsKKjS7GuM7YdY2tmFoSgTmjU236rCllosfKqqSvRKKbydoodfm07FdA4vtuZfNVXqldxCHXcChdKNPSyYWOHGhljc357PZwzF5+R60WQwH+RoYfDnnJry5JVd2hWQ5/nu8DFkl1Va3ccfwoVsS0MgW4nJTFp7dtgoHzmfhdMUJtLRf25TYsrdgfWBkk8xTSn5sKvxMaTAg+Ppj2vg7GKkcI6lAwfwdkCgS6Uh1XLkjhLadutppYduRHkc2BBXc2w2QnAKVE6Q5c7NSLVFwQ4ibaFXD4kBhpej9+wsqBPNXuPfnTZhlrifMik1dHSqqsvsMf57SF7/46y7RjuyBkSkI9DsvI9Cw3wl69ieHJKdgOCcv1lrlaIzOiME3j45CRX2L4irCjrLcFZsbkXprS67dlBbLAi9/exIPjEhBY0ubVZ2dvgmhMPoYcOx8De/Vc2u7iTfYzUwKM79OO2rQYijsSPRlCrCptADr/30BJtY+ZGTYgGtTSmkwmoOZZDBQn9PSNzEUpxXuf2VJTpFIrqNW8zuTO0LIdepa1wtyZENQtVO3I3tEyXoPrTYrdTYKbjSk54JLRD+0L5wmflVvuwaI7/1HpEVheFqkVUIsN8Iyc9VBTBtiv7WJJdur5Bc2nkBDi/gVeu/EMKwdkGA+SUoFGpY7Qf9r7kjct/Ja1WCJQY2aRusRGMtlqwXl6kdjbA1NjrArWChmT/4VwSCNCxq5zntoSgRmjUxBn4QwpEZ34U1wtrx65u5jwaKNuYSMxAr4R5Xi8qU9aGEK0W7gySVhga5dusKnLRUN9UnwNaVdy4+JBwOD/eMF+DAMBiWHiwYeb9zTD0v+d0b11X9YkFGkSGRHkBQRZLSbgpP7OxObfhnUPRzzxqXzJula0mLptpINQTlqp26VDg6qaZsrUXCjAU8voU1cS+vCacNT7ZfKWt2fZn0/3/sfLKzEqPRo3g56T/4VNLSI535wdVO4ZcFiozEGADdnxJhPjNxUTUJ4oOR7WHrhrt5Y8GWW4tVGliNBcqc45OgYifKVnashJwGYk1VcjUDjeaydnQCA/+q5W4Qffr7yM7LLshHdLQfx7GGcuHwMda01uFgBoAKwzOH1NcXDj00zTy2t/M29GBCfgnFLdyJM4We3ZFmfSCg3o7sGm6sCwEezhgoGedz7W1KyXFxs+sXyvK635dFKN3zlyJnC9iQU3GjAmVU+iXdxxokwLSYYI3tE8S5Dth1qlnp/PlwnLJTgGh5oxMubTsru1IP8O/ZyEkpQrm1stdoQkhsJ4ArlRQQZZS/RFcONBP15Sh/84q8/OVxvhhuJOna+Gn/aYL1hpaO44/NTXjluyYhBXUsdjl06ZrXb9YnLJ9Dcbl8/xmgwIsQnDS2NSR3TSmwa/EypMMA6wGhqClU9pQEAD45MwcyRKebvm1hwwBd4qNn3SmiKRDDv5Npo5H0r9kouF5c7/aLHnbH5/vZyuHMXb61RcOMgvUXtRN+cdSJcMWOw4A7KnOqGFiz4Uv2yY6GVO9WNrdit4CTa0Nyxl1NDSxuybEYuahpaEWazeic00BeHiqrM9TW4AMhR3EjQCxtPSk6hibHNFekeGYTIIPmb9Q5NiUBWsXBidTuqOpZbGwpw59q3wPgVocF0HizPdGSofygGxg3EwK4DO/ZYis9EALrjzvf3SbaDgfopDQBWgQ2gPPBwZN8r2ykSpcvFxQIoqekXPS6PtvzbP7YuC6cu1soamdTTUm5HUXDjID1G7US/nHUilHOVuWBdDk6J1J9xhNw8E+6xQleUJnSsUnr73n4orW3CluNlOF1mvexYyYabPkxHRpLJZiSIC0aULvkFxJdOA8rqowDAi7/sjSX/O4OdeZfQxlxCC3Otmu+1gKadsUkYv/bHDjTE4JaUIRiaOMi8WWRqRCoMjHV+jFQlYk6v+BC88s0p2e22JJaMqjTw0GLfK7nLxY+eq7YrRqg0pUDu8mhX5GTavkdqdBd8PvsmyVEcPS7ldhQFNw7SY9RO9MvZdSKErjKlOnEuDwYAb9ukKt9q7dl/a7OCycQC4SLBiJJpGMu9sYSCSLnBEotWtDLn0GI4i1d/2ohyQy4qw46iroUn+GQZ+LKJFkXwOpZdG5kI+FWHIiahK7oaI9AjMob3veSMxmQmheP5DSckg18DAxgYxmrZekSQEW9N7S/5HubXkLj/5IUa+DD8AbDcEXG5uVTPbzxut02ImpSCP0/pg8nL99iNOApNv2qdkyn1HpYXPlFBfljyvfpkbk9BwY2DPKmoEdEHV9aJ4K7kymqaRB/XOyHUbqWNbdv4EkQNjP0eOHrComOk59PZw9BmYuHDdNSwqWxokVUUkHuebRAjtGsNX7BkQsO1Kr6FFnsslQBMxzTf+jPXH+vn44fe0X1RV5eEyqoEGNk0+JlSYIB9srWJBY5fqMXxa7k9EUFGfDP/ZiRFWX8mqQTTiCAjss9Vi/4dOGGBRtTYjJzVNrbh+Y0nZAcDUqN8S74/I/EIeSPicpaL8+VFcQHUuoMluClN3vLoFzaeRK3NRq7c3wWAU3IyLUdpXt50UvI9LC98PGEpt6MouNHAn6f0xeTlu3mjdkJsuaJOhJJ9cQBg2f2DzFeRQm3jC8p6J4Si+Eo9rmq0+aOz5F26ih25V3ivbMUuTm7JsB4NkbpCDvSvRaPhkDlHpoUpQJuhlLdNBrYLYgJ64v7MW827Xd8YfSOMPh3H4atDJXhOwQhWVUMrJi3fjeyXxtvdx3fsLOvlSHnyjgxkJoVb1enhSI2m2E6VyF9ULkxODRsWLAKN/F2cnG1CuCrZfFtxaJGkrzYnU+5vW+o99L6U21G0caYGHNmIjHgHvdU44vtO8lHzPT16rgrPbzgh2jG42tCUCNErdD7cZ+fr+IWmDbi/axvbjjamFC2Gs2gzFCKoyzm0+xbiUv0l/vcyRV9bpXTtP7YHogMS8e2jt9iNtCgNTG19OnuYXVDGsQxapTYvtSRn41HbTRP5PsewlAicuFiDhhYlWVr2RqRFgWFgtULQ9piJ/QY69lkS3yaE48MAoYHWU5uW7yW1maSYJ+/IwKQBiYIblPKdT361Yi+yFNRT0nozS3ee62jjTBei1VKdm95qHBWU1+FAYaXsjlHNdNjS7/NwupR/byG1DAzQJyHUPMUi6zm4Np12/yDB/XjEcL/RyoYW0ZG05rZmnLh8At/n7cPGoi1oMRagxVAIlrk+1VfdDKAZMDAG+LQnwmhKs6ghkwofnsoxV5vaeadzlCYk28oqqcItGTFWdYfaWdYqyRSQl2hsOb0udR1sm1/IW09JYQAqhK8Cs+U0jFTu01PjM7BUxvQXwL8Vh+V7ObLC7L2teXhva575nMGCFTyfsGAxZ+1hxblvWuV96u1cJ4WCGwfRaqnOTS81jpRc7b85tR+6hgXIuvKyvUpTs7pIjpvTY8wjKHL3jTIBVqNHfEmdcnC/0dToLogIbkVO2SFsys9Bdlk2Dpw7gvyqXLSz1/IpLM6YDOsHI5tiHo15YvQdGNJtIOZ+elLW+/JdAGnx970hNtiuMi9H6SaklsGvkvxCZ31PxFj+PaXOy89vOOFQgM5XCFLo7wLYJ+nb4s4Z3P/mu6+hpQ1HFAQ2Wud96uVcJxcFNw6i1VKdl55G7ZRc7Q+XkSTJFyz1TQjF1EGJst7DAKCHwC7aHL5k3T9P6YtfLvtJ0W7YXHDCl9QphAWLdlSgxVCAz07+hJd2H0dRzUmcv1rM/3nYEET59URzQ3cY2VT4mXrAyCaCsSj5u2o7cDT5oux227YfkL5YeuqOG2ACi3/sKuAtOhgRZMS6g+cFvwu78sox9/Mj+GLOTYKdMreD+bLpg+y+J3KT4R0pBsjxYRj0SghRXAyxqKJe8rys1ZQqd+yk/i5SS7Hl5OcopeUiBT2d6+Si4MZBtFqq89LLqJ3cq2Ql30m+YOnExVrZnUJYkBEfzxqK5zeekJ2sC3TsSVWvMDm5vLYZ6w4WC/4NWLSjjblo3iSSS/Y1MR2f5WObnN2k0GSgJQV1V7t17K/EpsKHjYFvswGJgb6obWwTvAo/XFyF8EAjrjYJP8aW5QWQVLLt0q3Xp1K4lV+ciCAj/jY9EzNWHRR9jb1nK7DrzGW0s8DTE+x3SedG0fimGuQmw6uZqkmJCkJRxfXf1Kj0aDw1PgOTl+9V+DodbRI6L0sFTEpWAHLHTurvwt33zdELeG9rnqLPIwdXyuHVyX0cWqQglE+jl3OdEhTcaECsxgHxXnoZtZN7ldwrIcTcmYnRYkqhqqEVz/77GFb+1r5ycq/4EDw93r4dat/32X8fM/9vE5rRyhRd3/HaUIBWpggsY78tAVgDjGy3jrwYiz2WeoZ0RV5VPWzTFdtZFlUNrciQGJGqbmxVleDc0X75uMAmOSoQiyb2wk1pUfjtqgOynmu7O/k380ehoqFFdqcotdJGzf5Gqx/smNqw7ZyHJEfISqC1Dd75RlN6JYRg/ph0zP08S/B1BifLO3YjeEZAxf4uqdFdcHf/BKcEN4OTI8wBqZogQyqfRi/nOiUouNGAWI0DPc5FEm24a9TO9upK6sTDdcYnLtRi0t/2SCYBajGlAHQkfXLJukfPVeP5jR37LZ24WItJy+3bofR923EVLYazHZV8r43ItDLnAca+G2RYf/iZUq/VjUm7Nq3UHQbYb5OQd7le9H3FAhtO34QwLLgtA20mVvZu5wXldSirUbYJKACcq2jEFwfO4YsD51RVoLbcJR2ARSJyRwCldhSAL7gQ2p9saHKE+T24/891uHwJtHyrpWynYfi2pzhxoRZzP88S3MOMWzlYeKUej31xbdsCgc/HMKIfn5cW+TlWbQAwJCVC1kagYqTyaTxxhoKCGwd54lwk0Y4rC/KJXV3x5k4ACA7wRUG5dWctlQToyOoPW/sLKpAa3QVLvz8jWQlW6H1ZsGhnyq8FMGc7RmWYs2g38I/yGNgwiyXXHf/fl02wyo9xttV7i7B6b9G1JccZoo8tLK/DBz/m45DKCtBi21nIwZ2rVu0uwJYTZbyjFmpWxfAFF30SwgSXcNvi63ANTMcoxbrf3wQAktuNCOUe8e1hZvm7ZVlWcgp271l1u2g7mp9jaUhyBD6aOVTR+9vamXtZVh/mynOdFii4cZAnzkUS7biiIB9H7OqK78RjAv9ml2KBN3fVPjQ5AoeLq3i6J2UYyL8ASIsJxs3pEdhZcBSNzFmrPZZMDP9oia8pDn5sWsfS62vTSj6IBAMVl9VOwB0vsSma1zafdnWzeL0u0g41q2J4l4IXVmJUerS5bo7Q70VqV2/uO6N2uxFuDzOhCtRyRxHVnN/l5udw9z2/4bhVMMjJTArH13PVj9jIXWHJfUZXnuu0QMGNgzxxLpJoz9nVPqUCBG76576Ve3GkuEpWQiQ3qgLwn+iC/Hwc2i0b6FiZVVTBP81jQhNaDYX4y/6TaGDzkVOWg+OXj6PJn2erCNYHRra73YiMAfr+fXHH55v5owA4NsLiTkpHouVU7RUrLCcVXDy2Lgufz77J4anVNhNr1Q7LKTk55FRKFiKVn8Pd19bOPzFm5GmkknbIXWFp24d5SmVjCm4c5IlzkcTzyBkhZFlWURLrovXH8d/jZVg2PZP3RNfYKh7Y+PsyaG4TjqJG9ohCRJARz3ydj3bUXFuldH3FUhtzAWBY/OWI9fN8mSD4tKVYVPTtyI9hoG2hsMykcJTXNeF8lfi+W1r404bj+Pzhm1BUUYenvz4qmdej1MgeUQDAe4WvJbkjFXJHtIUKDUpdNJ66WKvJ1KoPw2B77mVEBvnZ7Q4eEdSxjxZfaMGd3yOCjHY1hYSm8NQEQAXldYKFDw9ajGDJKbBn+f6sjOXlnt6HUXCjAU+biySeR84IodAIiZg9+Vcw+5NDvEmbUjmNN3QNsasozIJFG3MJGYlXkJhcgz5/fRqXmn5GeyB/p+vPROKmpCEY2X0wMuMy8flPDI4VB8DEajOtlBIVhJLKBquRLAYdO3xvmD8KBeV1srcgcATXGTe0tCFf48AGkD5WWpEzEl1cUY/HrxWkExLJExRwLPPIhAo6mliIjiSlxQRjZI8owWDPgI7poZkfCy+br2lshcHAwMQzDGq7mawl2yk8Ryr7HigUD1YPXBt9FZuy/uv0gbw1q6R4eh9GwY0GPG0ukriXmiu4tJhgRNgkQHIiri3/VLNNXDvLKi7nzjl2oRKtzDmLTSLPXtuWoB4XK4Cd+6498Fqc4mtKsJpS8jP1gA8iUJQLdDfFYNoNNyCnSHqvH46cQoGWdVM4LDp205656iCWTc9EZrcwZMvYPNIRjib9SuHbjsAZXt50knc0wHJl1dQP9ogWYQwN8MXrm08jS+B7Z5lHNuOj/aKJvbYjQJa/KbGfQ9i11VJiTCxgsnkRAzoSmsW2eLCdwnOssq94kM9CegpwzieHkVVSbXWf1Ko6bn+ygvI6ZJ2r8sg+jYIbDXnKXCRxD0eu4ArK6wS3FahqaLUqA787v1x2ETK5TGg0r1LiknxbmGKA4enEWF+khN2IcN8MFJfFXgtmUmGA8OjT7rxyFFdIL7G2NDg5Ah/NGopb39mOaomOig/XwTS1ObaJY2fyU145Zqzaj/+b0s9uGkeu2qY20YDaMo/sr9MzRUfWGlva8MtlP1kV5eubGIr5Y9JFAz6lW3RwTAAOFVfJ2uKBmyp2ZDXt8NRI0fe4SSSnjcO3Co/7xtsWLOSmovolhsmebtMrCm4IcRFHruDk5jAsm56JGav2Ky5Zb6kdVRb5MR3F8NqYUoCxj5gYNshmNCYNRjYJbJMRVYBdITwhJgDFlcpqvFQ3tqKool5VYAOoL2vfmbHoqBUjZzdtRxVV1GNsz1iBnMaOnbrnfW4//cXVsnF227SYKpbKYUqLCcaItCjeQI0rIqhmxJbTOyHU6lyhZLpN7yi4IcQFHK2HJHdVXliQEU+N74kHVx8SfTwAsDCBNZQhJqoU+VUnzCMy7Qz/VbUPGwU/Uxr82TT0jx2I8sp41DdFuW3Zdd7lOixY59xOzFOM7BEFX4NBUQE4veO+07zFAAONqFY5+qIFqS0e1O6kzoevynffhFAsmngjAPFFLYOSw0UXGSybPgiAdVVob6ndRsENIS7gaD0kJavy+AIhFq1oYYo7ppPMFX0LwTKNOFcHWC1EYhn4sokWIzJc/ZgwALhWmO4GTF6+x+3VZJSO9nir1jYTVsyy7wRtDU2JQHpsMNYdPOfC1ilj+522zWn0Yay3j1Dz2oB0JWC+1VJytniQu5N6Zvdw88iO2G+f+/xCVb6fGp+BaUO6obGlzWoKynYURuy84en7SPGh4IYQF9CiHpLcVXlRIe3o2f08jlzMQhNztmOLAuYcwNgv7Q7wDUDf2L6orklEdXUifNvTYGRTYECA3WP7JoTijXv6oX9SOLbnXpZsL3GdQ8VVqGxowSuTeovmqLz9qwHYX1Ch6+CG+07bJglz/zny3ROrBDw6IwZPj7/BvMdWZJCf5O9NzmIS/pEnXxwurjKPsMrJZ+Gr8r0rr9zqdYemRGDWyBT0SQiTHYDZ8pbabRTcEOICWtRDsj2RJkcGwd+/BrvPf4/ssmxkl2UjpywHBVUFHU+wOU/6MaEYnjQIQxMGYWDcQGTGZ+LG6Bvha/BFTUOr5MleanSIuNd+GSum9hdUSCapaqFXXAgeujkVJy/WYM3eYkXPfWr8DbzfRbmbOMohd4Wr3FWwtotJbAMzy9f5YHs+soqrrZ4vlM9iWQdITn5YVnE1Ao3nsXZ2guLPyvGW2m0M60g2kgeqra1FWFgYampqEBoqN92REMcJBRByViC0m9qxPf8odhYdwuWmXBTVnkR2aTbKG/hPeN3DumNg3ECkhPZBjH9PjEkdhlGpN4KR2O1PSTmDmasOOpTj0SOmC86W8ydcqt1IUK5gfx/UNTtWfVlvnrrjBlTUN4sGE0/dcQP6dgvDX7eecfryd6Bj5ZIBDI5dkP9efRNDcfriVd6Olev81X73bF9Ha1IrIqXqKm1/eoxgUT4luNdRy5FzlTMp6b8puCHExaQCiKa2Jpy4fALZpR0jMYcuZiG79CjaWPv8EgNjQK/oXhgYN7BjNCYuEwPjBiIqKMrpn4PvBDiyRxRY1vG6K5abKT689pCiystyeWOAo4QrP79QjSaluE6b77un5nU4ampP8eELuiwDqu25l0WT/Vc/OBRje8Y6fOHAvY6j9Fa7TUn/TdNShLiY5RB2VWMVcspyzFNK2WXZOF1+Gu2sfafDsP4wsinmAnh+pjSM7TEYK+4foepqSs4JXewxYsPd3G1RQX5YoqIeSmNLm/k95o1Nl7X6S6nOHNgA1z9/34RQPDo2Hf5+PrhU04Q/rj+u+XvVNLRiaHIEGlvbcepireCWBr3iQ2QV7bP97kV18cOS/8n/nnGv40jtKVtyVhnJyWeR2vRTDq3yYjy5dhsFN4S4AMuyOF973iqIySnLQVF1Ee/jowKjkBmfidTQPthw0A9+pjT4sglg4GP1uP35VxXXnpBzQldy0uc7AVreJpXkyudwcZX5cxkUPZModepiLb44eM5cddcZuOJ3Yrr4+2DRxF6YseqA4GM++DEfg5IizN9By++ZkhVVXOc/97Msu1HGXXnleOSzI1j3+5vkfDQzOauMhOv2XM9ncSRh2tPyYpyJghtCNNZmasOZijPILr2e5JtTloOKRv6pmpTwFPN0UmZcJjLjM5EYkgjm2qZ+3+8TPklzZf2V1J6QU0xw3udZdvvy7Morx9zPj+CLOfJP+gXldfj22EXZj+ewuP65qH6wek/ekYHYkAAsEhmNsfwOCSWTciKubVvQ7oRkhrrmNvx9V4Ho+2eVVFt9T4VWVAGQDCIKyusEp0/3FVQI/qaERjPlrjKSWr0k9Tqfzh6GNhPLOyrqiv2gtJrCczYKbghxQENrA45fOm41InPs0jE0tdnvNO1r8EWv6F7IjM80BzMD4wYiPCBc8PXlrgyRW3tCztA5y7KCGw7uPWt/0uc72TmaEMk5cbEGn+wpcug19MoVOS+DukcgMTxQ1mMtq1wL5bPUNHRsJmm5cZOvgUGbBvt9cJthfjN/FBpb23jzrLjv6dFz1XbbP9iOLIoFEdUNLfjDp4dF27P/2qaUHKnRTLmrjKRWL4lt+jmyRxRuyYgx/9uVexoKff6nxmegsqFVd8EOBTeEyFTRUNGx5Lo0GzmXcpBdmo3cilyYWPuxhWC/YAzoOsAqybdPbB8E+NrXjxEjd78ouXPsUkPnj32RhamDuok+htuJWOxkzzc6pMaHO8/ipMBWEgYGCPLz3KTgXvGhuKt/PF755pTgYzK7hyPA10d1gnabiRUt4W+J+w6xYNHQwr/xpQmw2yWbZYGhyRF4YGQK1uwt4p1+slz9JrTTN6eioUUyz+r5jcftar7Yjj6KBREzVx2U3J39TNlVq0BezoinkpoyYvksQnnEfLe7Ki+G7/Pb1tnRw4oqDgU3GvKU4ToijmVZFNcUd4zEWEwtnavlL3wW2yW2Yzrp2pTSwLiBSI9Mh4HRJltE7Epa6Ry71EjQqYu1aG4Tr0vCnV+FTvZarm46LrJHlon17KTgrOJqsGxHQq9tki23+/TXc0dKLh8WwwUsYhUAbL9DC9blCO7YzaedZXGouApv3zcAX88diWPnqvGnDcetEoMtO/jZnxwS3Tjzgx/z8cIve4m+J9/eaULbA/DVoJEzorh6bxFW7y0yj07I2ZJAaU0ZPmqny5xJ7t9MT/tPUXCjAS0z7olrtba34ucrP1sl+eaU5aCqif/k2yOih9W0UmZcJuJD4p3aRu6EKdVpyCE1EmQCkCdxRXtTWpTo9JYzlm3rie1Oymq1s6xgJ3+zxVJ4qdE2MS9vOomnxmcITjMCwKDu4eb3cmSlDjet1T8pHJsX3CLYwf9r7kjct3IvjhRX8f4ds0qqsfT7PMEpHrkrqoQuNpX+PffkX0FlQ7PoY2ynhR0ZTXH29gdqLsLl/s30tP8UBTcacGS3Z+I69S31OHrpqNWIzInLJ9Dcbn/iMhqM6Bvb16p+zIC4AQj1d19tJKlOQy45O4f3ig/B6dKrdrdzOxF35u0XtAhs+BiYjl2an51wI9pZFpUNLQ5X5JXTMc8bl26+CHMkkOJGia5X1RV+7EczhwqO8HEd5DfzRwGA3RQPt6+ZkMggI2auOqhZheN2lhX9rQD2q7gc4aztDxy5CFc6Bq2H/acouHGQt+yg6m0u1182F8HjtibIq8gDC/ueKcQvxGql0sC4gegd0xt+Pn5uaLk0R+fYw4KM+OtvMkWnOt66t79d3RDLwnq0/YL2TGzHdMvMjw+ab+P+5nLyrvjI6ZgtO0s1x5Wb1orgCSo4tp2onPpFFQ0tglM8Yom7S7/PE73YlJvHZqtvQihOl16VtYrLEc7a/kDNRbjahQF62H+KghsHecsOqp6KZVkUVBVYBTE5ZTm4eJV/+XF8cLxVIJMZl4nUiFTN8mM8hdQJtH+3cMkVHWo7XCIf1/ksm57pUC4TX8fM11lyx/WnvHKeywB+trtPi30Oy05U7ggFXzAvlLj71PgMTF6+1+61bC821fw937inH17/7pToaJNWF7NKN7uUovYiXOnCAD3V2aHgxkHesoOqJ2hpb8Gp8lNW00pHLx1FbbP9lSkDBhlRGdcDmWs5Ml2Du7qh5fok5wQqNkokZ3pr8dR+ACBaZ0UuA9ORZBto9HXKflN6xHU+lQ0t+PqRkej/yv9Q22S/kqmLnw/qW4STq9+4p5/smijLpmdKJv1ytVa4oFcqV4evE3VkhEIocVdqutSywvHXj4zEfSuu5f6IPMcc8CeFS442aXUxq0VisiU1F+FSx3TljEH44uA5l9fZkYuCGwd5yw6qelPbXItjl45ZTS2dLD+JlvYWu8f6+fihX2w/qxGZ/l37I9gv2OXt1uuKOb52OXoClTO9lRgeiHaWxdCUCGQVV9snhyaESE6bcG5Ovz4t5si+Qu7WNyEURh8Djp2vkR2gFVV01B/iC2wAoL6lHaEBvrz3RwQZ0T9JfCTOUliQEf+aOxL3LN+N7HP2G16OSLOutQLIz9Wx7UQdHaGwDb6lxl99DdaJQB/NGmr3/rb7YCkpsKf1xaxWy7zVtFvqmPr7+bi0zo5SFNxoQOshxM6m9Gqp3bRSfmU+72PDA8KtasdkxmXixugbYfRx76o0va6Yk9MuR06gQsG9AR2dpGX+CF+nITSNwLEdIeBYnlSFdrhOigjAuSr7YorO4O/DoFlm2d5TpbUY0C0Mo9KjZQdokUFGLPgyW/QxQoFPVUOrecREybFe8+BwwZ2hbcnN1bHtRLUeoZCqZm1bbFDo/aWmYz3tYlZNux2ZNtQD2hVcQ3qNYPXCxJqQX5lvnlbiCuFdqr/E+/huod2sppQy4zORHJYMRqxoh5tI7Qbsze3i26GZr0y/D8NgUPdwzBuXjpSoLh31hCob8MGP+cgqsR/VkdNGR2rAaGVocgQ+mjUUD6w+iOxz1Yqe9+Ive6OioQUpUV3w8qaTgscKgEP5TY7sEi33vCa2k7WrfgtS3wfb3cDV4Pu+y7mQcfeorpp26+28RruCu4leI1h3aG5rxsnyk1ZF8I5eOoq6FvuN+QyMAT2jenasVOo60LxiKToo2g0tV06vK+Zc1S7bq18fhrEasbF830PFVYgMMuLlTSet2uZrYGCZwRoa6Iv/m9JX8r2lhs57xYXgdJn9knagIwCraWi1K57XJyEUj9zaQ7Darvmx13KAvn5kJAAgwOgj+Fg+R4qrsOT7M5LVbaVGtwAgMylcNLByZLpE7nlNrNikq0ayXTGyonS0SS+jumpGyTx5VoKCG+Kw6qZqHC07ajWtdKr8FNpM9sPkAb4B6N+1vzmIyYzLRL+u/RBk9NylxXpdMefqdnGdoFRS5/MbTtjV0LGdLqhtbMPzG09IXh1KDZ2/cFcvPLou22o6LNjfBx/+dgj6JIbZnbhvtuh07hqQYO4Iorr42S2Nt8wBEqsqK8R201O1SbJAR2Dl7ukS2/Zze065eqTCVR2y3KBPb3XQlFyEaz1t6EoU3BDZWJbFxasX7fZXKqwu5H18ZGCk1WqlzPhM3BB1A3wN3vW10+uKOXe1SyqpU6y6LEfu6JLUlfrfdxWittE6yG5sMWHlrgKsnT1M8sRt2RGIPdaR4ndS1W3l5LPsK6jAN4/yF71z9VW2u0ew9dQh63VUVyl3H1M1vKuXIZppN7XjTMUZq20JssuycaWBv+ZBcliyXSG8pNAkXebHaE2vSYbuapdUUqcSckaXHK15ouTELfRYR4oaSgWZcmsKVdQLF73rjPTQIet1VLczoOCGoLG1EScun7AakTl26RgaWu1/mD6MD3rF9OrYlsAiPyYyMNINLdcPvc5Nu6NdWlYvljO65GjNEy1IBZKNLW129VSUBJlyis7pffVKZ6TXUd3OgIKbTqaysdJutdLPV35GO2tfACzIGIQBXQdY7a/UN7YvAo2Bbmi5vulpKNzd7ZLq6AFIFuFTM7qkdDpH645FKpB0JMgUKzrn7hFCIkyvo7qdAS0F91Isy+Jc7Tm7/ZVKakp4Hx8TFGO3WikjMgM+BmUrQAgBxJedAvYdvW0NHK1Wk7hjKatYIOlokKl2GTJxHzpm2lHSf1Nw4wXaTG3IvZJrNa2UU5aDysZK3senhqeaVypxIzIJIQmdIj+GuJaSjt4Zo0ve2rHobYSQSKNj5jgKbkR4enDT0Npgty3B8cvH0dRmX4nV1+CL3jG9rQrhDYgbgPCAcNc3nBA3oo6FEM9HRfy8xJWGK1ZF8LLLsnGm4gxMrP16lGC/YLsk3z4xfeDv6++GlhOiL5RkS0jnQsGNDrAsi6LqIqsgJrs0GxeuXuB9fFxwnFUgkxmXiR6RPWBgpCqMEEIIId5PF8HN8uXL8c4776CsrAwDBgzAsmXLMGyYcLLf119/jRdffBFFRUXIyMjAW2+9hV/84hcubLF6re2tOH3ltNW0Uk5ZDmqa7Tf+A4CMyAzrjSLjMxEXHOfiVhNCCCGew+3BzVdffYWFCxdi5cqVGD58ON5//31MmDABubm5iI213+ht7969mD59OhYvXoxf/vKX+OKLLzBlyhRkZWWhb1/pvWhcqa6lzpwfw61WOnH5BFraW+weazQY0Te2r1URvAFdByDEP8QNLSeEEEI8l9sTiocPH46hQ4fib3/7GwDAZDIhKSkJjz32GP74xz/aPX7atGmor6/H5s2bzbfddNNNGDhwIFauXCn5fs5KKK5pqsH+8/ut9lfKq8gDC/s/b6h/qPVoTFwmesX0gp+Pn2btIYQQQryJxyQUt7S04MiRI1i0aJH5NoPBgNtvvx379u3jfc6+ffuwcOFCq9smTJiAjRs38j6+ubkZzc3N5n/X1HRM/9TWSu9vo8QPZ3/Ar/75K7vb40Li0D+2PwZ0HYB+Xfuhf9f+SA5PtsuPaapvQhPsVzwRQggh5Hq/LWdMxq3BzZUrV9De3o6uXbta3d61a1f8/PPPvM8pKyvjfXxZWRnv4xcvXoxXX33V7vakpCSVrVam7Nr/fY/vXfJ+hBBCiDe7evUqwsLCRB/j9pwbZ1u0aJHVSI/JZEJlZSWioqI0L1pXW1uLpKQknDt3ziNr6Ejx9s8H0Gf0Bt7++QD6jN7A2z8foP1nZFkWV69eRUJCguRj3RrcREdHw8fHB5cuXbK6/dKlS4iL418RFBcXp+jx/v7+8Pe3rvUSHh6uvtEyhIaGeu2XFfD+zwfQZ/QG3v75APqM3sDbPx+g7WeUGrHhuLUwip+fHwYPHoxt27aZbzOZTNi2bRtGjBjB+5wRI0ZYPR4Atm7dKvh4QgghhHQubp+WWrhwIWbNmoUhQ4Zg2LBheP/991FfX48HH3wQADBz5kwkJiZi8eLFAIDHH38ct956K5YuXYq77roLX375JQ4fPowPP/zQnR+DEEIIITrh9uBm2rRpKC8vx0svvYSysjIMHDgQW7ZsMScNl5SUwGC4PsA0cuRIfPHFF3jhhRfwpz/9CRkZGdi4caMuatz4+/vj5ZdftpsG8xbe/vkA+ozewNs/H0Cf0Rt4++cD3PsZ3V7nhhBCCCFES7QZESGEEEK8CgU3hBBCCPEqFNwQQgghxKtQcEMIIYQQr0LBjUaWL1+OlJQUBAQEYPjw4Th48KC7m6Ta4sWLMXToUISEhCA2NhZTpkxBbm6u1WPGjBkDhmGs/nvkkUfc1GJlXnnlFbu233jjjeb7m5qaMH/+fERFRSE4OBj33nuvXeFIvUtJSbH7jAzDYP78+QA88/jt2rULd999NxISEsAwjN1+cizL4qWXXkJ8fDwCAwNx++23Iy8vz+oxlZWVmDFjBkJDQxEeHo7Zs2ejrq7OhZ9CmNjna21txXPPPYd+/fqhS5cuSEhIwMyZM3Hx4kWr1+A77m+++aaLP4kwqWP4wAMP2LX/zjvvtHqMno8hIP0Z+X6XDMPgnXfeMT9Gz8dRTv8g5xxaUlKCu+66C0FBQYiNjcUzzzyDtrY2zdpJwY0GvvrqKyxcuBAvv/wysrKyMGDAAEyYMAGXL192d9NU2blzJ+bPn4/9+/dj69ataG1txfjx41FfX2/1uDlz5qC0tNT839tvv+2mFivXp08fq7bv3r3bfN+TTz6Jb7/9Fl9//TV27tyJixcvYurUqW5srXKHDh2y+nxbt24FANx3333mx3ja8auvr8eAAQOwfPly3vvffvtt/PWvf8XKlStx4MABdOnSBRMmTEBT0/UNaWfMmIGTJ09i69at2Lx5M3bt2oXf//73rvoIosQ+X0NDA7KysvDiiy8iKysL69evR25uLiZNmmT32Ndee83quD722GOuaL4sUscQAO68806r9q9bt87qfj0fQ0D6M1p+ttLSUnz88cdgGAb33nuv1eP0ehzl9A9S59D29nbcddddaGlpwd69e/HJJ59gzZo1eOmll7RrKEscNmzYMHb+/Pnmf7e3t7MJCQns4sWL3dgq7Vy+fJkFwO7cudN826233so+/vjj7muUA15++WV2wIABvPdVV1ezRqOR/frrr823nT59mgXA7tu3z0Ut1N7jjz/O9ujRgzWZTCzLevbxY1mWBcBu2LDB/G+TycTGxcWx77zzjvm26upq1t/fn123bh3Lsix76tQpFgB76NAh82P++9//sgzDsBcuXHBZ2+Ww/Xx8Dh48yAJgi4uLzbclJyez7733nnMbpxG+zzhr1ix28uTJgs/xpGPIsvKO4+TJk9lx48ZZ3eZJx9G2f5BzDv3Pf/7DGgwGtqyszPyYFStWsKGhoWxzc7Mm7aKRGwe1tLTgyJEjuP322823GQwG3H777di3b58bW6admpoaAEBkZKTV7Z9//jmio6PRt29fLFq0CA0NDe5onip5eXlISEhAWloaZsyYgZKSEgDAkSNH0NraanU8b7zxRnTv3t1jj2dLSws+++wzPPTQQ1abxXry8bNVWFiIsrIyq+MWFhaG4cOHm4/bvn37EB4ejiFDhpgfc/vtt8NgMODAgQMub7OjampqwDCM3V55b775JqKiopCZmYl33nlH06F+V9ixYwdiY2PRs2dPzJ07FxUVFeb7vO0YXrp0Cd999x1mz55td5+nHEfb/kHOOXTfvn3o16+fuVgvAEyYMAG1tbU4efKkJu1ye4ViT3flyhW0t7dbHSQA6Nq1K37++Wc3tUo7JpMJTzzxBEaNGmVVBfr+++9HcnIyEhIScOzYMTz33HPIzc3F+vXr3dhaeYYPH441a9agZ8+eKC0txauvvopbbrkFJ06cQFlZGfz8/Ow6jK5du6KsrMw9DXbQxo0bUV1djQceeMB8mycfPz7cseH7HXL3lZWVITY21up+X19fREZGetyxbWpqwnPPPYfp06dbbUi4YMECDBo0CJGRkdi7dy8WLVqE0tJSvPvuu25srXx33nknpk6ditTUVJw9exZ/+tOfMHHiROzbtw8+Pj5edQwB4JNPPkFISIjdtLenHEe+/kHOObSsrIz3t8rdpwUKboio+fPn48SJE1Y5KQCs5rj79euH+Ph43HbbbTh79ix69Ojh6mYqMnHiRPP/7t+/P4YPH47k5GT885//RGBgoBtb5hyrVq3CxIkTkZCQYL7Nk49fZ9fa2opf//rXYFkWK1assLpv4cKF5v/dv39/+Pn54Q9/+AMWL17sEWX+f/Ob35j/d79+/dC/f3/06NEDO3bswG233ebGljnHxx9/jBkzZiAgIMDqdk85jkL9gx7QtJSDoqOj4ePjY5cJfunSJcTFxbmpVdp49NFHsXnzZmzfvh3dunUTfezw4cMBAPn5+a5omqbCw8Nxww03ID8/H3FxcWhpaUF1dbXVYzz1eBYXF+OHH37Aww8/LPo4Tz5+AMzHRux3GBcXZ5fk39bWhsrKSo85tlxgU1xcjK1bt1qN2vAZPnw42traUFRU5JoGaiwtLQ3R0dHm76U3HEPOTz/9hNzcXMnfJqDP4yjUP8g5h8bFxfH+Vrn7tEDBjYP8/PwwePBgbNu2zXybyWTCtm3bMGLECDe2TD2WZfHoo49iw4YN+PHHH5Gamir5nJycHABAfHy8k1unvbq6Opw9exbx8fEYPHgwjEaj1fHMzc1FSUmJRx7P1atXIzY2FnfddZfo4zz5+AFAamoq4uLirI5bbW0tDhw4YD5uI0aMQHV1NY4cOWJ+zI8//giTyWQO7vSMC2zy8vLwww8/ICoqSvI5OTk5MBgMdlM5nuL8+fOoqKgwfy89/RhaWrVqFQYPHowBAwZIPlZPx1Gqf5BzDh0xYgSOHz9uFahywXrv3r01ayhx0Jdffsn6+/uza9asYU+dOsX+/ve/Z8PDw60ywT3J3Llz2bCwMHbHjh1saWmp+b+GhgaWZVk2Pz+ffe2119jDhw+zhYWF7KZNm9i0tDR29OjRbm65PE899RS7Y8cOtrCwkN2zZw97++23s9HR0ezly5dZlmXZRx55hO3evTv7448/socPH2ZHjBjBjhgxws2tVq69vZ3t3r07+9xzz1nd7qnH7+rVq2x2djabnZ3NAmDfffddNjs727xa6M0332TDw8PZTZs2sceOHWMnT57Mpqamso2NjebXuPPOO9nMzEz2wIED7O7du9mMjAx2+vTp7vpIVsQ+X0tLCztp0iS2W7dubE5OjtXvkltdsnfvXva9995jc3Jy2LNnz7KfffYZGxMTw86cOdPNn+w6sc949epV9umnn2b37dvHFhYWsj/88AM7aNAgNiMjg21qajK/hp6PIctKf09ZlmVramrYoKAgdsWKFXbP1/txlOofWFb6HNrW1sb27duXHT9+PJuTk8Nu2bKFjYmJYRctWqRZOym40ciyZcvY7t27s35+fuywYcPY/fv3u7tJqgHg/W/16tUsy7JsSUkJO3r0aDYyMpL19/dn09PT2WeeeYatqalxb8NlmjZtGhsfH8/6+fmxiYmJ7LRp09j8/Hzz/Y2Njey8efPYiIgINigoiL3nnnvY0tJSN7ZYnf/9738sADY3N9fqdk89ftu3b+f9Xs6aNYtl2Y7l4C+++CLbtWtX1t/fn73tttvsPntFRQU7ffp0Njg4mA0NDWUffPBB9urVq274NPbEPl9hYaHg73L79u0sy7LskSNH2OHDh7NhYWFsQEAA26tXL/aNN96wCgzcTewzNjQ0sOPHj2djYmJYo9HIJicns3PmzLG7SNTzMWRZ6e8py7Ls3//+dzYwMJCtrq62e77ej6NU/8Cy8s6hRUVF7MSJE9nAwEA2Ojqafeqpp9jW1lbN2slcaywhhBBCiFegnBtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV6FghtCCCGEeBUKbgghhBDiVSi4IYQQQohXoeCGEEIIIV6FghtCiK4wDIONGze6uxmEEA9GwQ0hxGXKysrw+OOPIz09HQEBAejatStGjRqFFStWoKGhwd3NI4R4CV93N4AQ0jkUFBRg1KhRCA8PxxtvvIF+/frB398fx48fx4cffojExERMmjTJ3c0khHgBGrkhhLjEvHnz4Ovri8OHD+PXv/41evXqhbS0NEyePBnfffcd7r77brvn7NixAwzDoLq62nxbTk4OGIZBUVGR+bY9e/ZgzJgxCAoKQkREBCZMmICqqioAQHNzMxYsWIDY2FgEBATg5ptvxqFDh8zPraqqwowZMxATE4PAwEBkZGRg9erV5vvPnTuHX//61wgPD0dkZCQmT55s9d6EEP2h4IYQ4nQVFRX4/vvvMX/+fHTp0oX3MQzDqHrtnJwc3Hbbbejduzf27duH3bt34+6770Z7ezsA4Nlnn8W///1vfPLJJ8jKykJ6ejomTJiAyspKAMCLL76IU6dO4b///S9Onz6NFStWIDo6GgDQ2tqKCRMmICQkBD/99BP27NmD4OBg3HnnnWhpaVHVXkKI89G0FCHE6fLz88GyLHr27Gl1e3R0NJqamgAA8+fPx1tvvaX4td9++20MGTIEH3zwgfm2Pn36AADq6+uxYsUKrFmzBhMnTgQA/OMf/8DWrVuxatUqPPPMMygpKUFmZiaGDBkCAEhJSTG/zldffQWTyYSPPvrIHHytXr0a4eHh2LFjB8aPH6+4vYQQ56ORG0KI2xw8eBA5OTno06cPmpubVb0GN3LD5+zZs2htbcWoUaPMtxmNRgwbNgynT58GAMydOxdffvklBg4ciGeffRZ79+41P/bo0aPIz89HSEgIgoODERwcjMjISDQ1NeHs2bOq2ksIcT4auSGEOF16ejoYhkFubq7V7WlpaQCAwMBA3ucZDB3XXyzLmm9rbW21eozQc+WaOHEiiouL8Z///Adbt27Fbbfdhvnz52PJkiWoq6vD4MGD8fnnn9s9LyYmxqH3JYQ4D43cEEKcLioqCnfccQf+9re/ob6+XvbzuACitLTUfFtOTo7VY/r3749t27bxPr9Hjx7w8/PDnj17zLe1trbi0KFD6N27t9X7zJo1C5999hnef/99fPjhhwCAQYMGIS8vD7GxsUhPT7f6LywsTPbnIIS4FgU3hBCX+OCDD9DW1oYhQ4bgq6++wunTp5Gbm4vPPvsMP//8M3x8fOyek56ejqSkJLzyyivIy8vDd999h6VLl1o9ZtGiRTh06BDmzZuHY8eO4eeff8aKFStw5coVdOnSBXPnzsUzzzyDLVu24NSpU5gzZw4aGhowe/ZsAMBLL72ETZs2IT8/HydPnsTmzZvRq1cvAMCMGTMQHR2NyZMn46effkJhYSF27NiBBQsW4Pz5887/oxFC1GEJIcRFLl68yD766KNsamoqazQa2eDgYHbYsGHsO++8w9bX17Msy7IA2A0bNpifs3v3brZfv35sQEAAe8stt7Bff/01C4AtLCw0P2bHjh3syJEjWX9/fzY8PJydMGECW1VVxbIsyzY2NrKPPfYYGx0dzfr7+7OjRo1iDx48aH7u66+/zvbq1YsNDAxkIyMj2cmTJ7MFBQXm+0tLS9mZM2ean5+WlsbOmTOHrampcerfihCiHsOyFpPZhBBCCCEejqalCCGEEOJVKLghhBBCiFeh4IYQQgghXoWCG0IIIYR4FQpuCCGEEOJVKLghhBBCiFeh4IYQQgghXoWCG0IIIYR4FQpuCCGEEOJVKLghhBBCiFeh4IYQQgghXuX/Af/fJ2k48OY3AAAAAElFTkSuQmCC",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "diabetes.plot.scatter(x = 'Glucose',y = 'DiabetesPedigreeFunction')\n",
    "plt.plot(x,y,'-g')\n",
    "plt.ylim(0,diabetes['DiabetesPedigreeFunction'].max()*1.1)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "id": "8f566ff9",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.640108Z",
     "iopub.status.busy": "2024-04-20T22:40:59.639584Z",
     "iopub.status.idle": "2024-04-20T22:40:59.647939Z",
     "shell.execute_reply": "2024-04-20T22:40:59.646480Z"
    },
    "papermill": {
     "duration": 0.024581,
     "end_time": "2024-04-20T22:40:59.651018",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.626437",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "diabetes['pred'] = diabetes['Glucose']*w*b"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "id": "dba646d4",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.676643Z",
     "iopub.status.busy": "2024-04-20T22:40:59.675322Z",
     "iopub.status.idle": "2024-04-20T22:40:59.698447Z",
     "shell.execute_reply": "2024-04-20T22:40:59.697296Z"
    },
    "papermill": {
     "duration": 0.038861,
     "end_time": "2024-04-20T22:40:59.701275",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.662414",
     "status": "completed"
    },
    "tags": []
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
       "      <th>Pregnancies</th>\n",
       "      <th>Glucose</th>\n",
       "      <th>BloodPressure</th>\n",
       "      <th>SkinThickness</th>\n",
       "      <th>Insulin</th>\n",
       "      <th>BMI</th>\n",
       "      <th>DiabetesPedigreeFunction</th>\n",
       "      <th>Age</th>\n",
       "      <th>Outcome</th>\n",
       "      <th>pred</th>\n",
       "      <th>diff</th>\n",
       "      <th>cuad</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>6</td>\n",
       "      <td>148</td>\n",
       "      <td>72</td>\n",
       "      <td>35</td>\n",
       "      <td>0</td>\n",
       "      <td>33.6</td>\n",
       "      <td>0.627</td>\n",
       "      <td>50</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>-0.627</td>\n",
       "      <td>0.393129</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1</td>\n",
       "      <td>85</td>\n",
       "      <td>66</td>\n",
       "      <td>29</td>\n",
       "      <td>0</td>\n",
       "      <td>26.6</td>\n",
       "      <td>0.351</td>\n",
       "      <td>31</td>\n",
       "      <td>0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>-0.351</td>\n",
       "      <td>0.123201</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>8</td>\n",
       "      <td>183</td>\n",
       "      <td>64</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>23.3</td>\n",
       "      <td>0.672</td>\n",
       "      <td>32</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>-0.672</td>\n",
       "      <td>0.451584</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1</td>\n",
       "      <td>89</td>\n",
       "      <td>66</td>\n",
       "      <td>23</td>\n",
       "      <td>94</td>\n",
       "      <td>28.1</td>\n",
       "      <td>0.167</td>\n",
       "      <td>21</td>\n",
       "      <td>0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>-0.167</td>\n",
       "      <td>0.027889</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>0</td>\n",
       "      <td>137</td>\n",
       "      <td>40</td>\n",
       "      <td>35</td>\n",
       "      <td>168</td>\n",
       "      <td>43.1</td>\n",
       "      <td>2.288</td>\n",
       "      <td>33</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>-2.288</td>\n",
       "      <td>5.234944</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Pregnancies  Glucose  BloodPressure  SkinThickness  Insulin   BMI  \\\n",
       "0            6      148             72             35        0  33.6   \n",
       "1            1       85             66             29        0  26.6   \n",
       "2            8      183             64              0        0  23.3   \n",
       "3            1       89             66             23       94  28.1   \n",
       "4            0      137             40             35      168  43.1   \n",
       "\n",
       "   DiabetesPedigreeFunction  Age  Outcome  pred   diff      cuad  \n",
       "0                     0.627   50        1   0.0 -0.627  0.393129  \n",
       "1                     0.351   31        0   0.0 -0.351  0.123201  \n",
       "2                     0.672   32        1   0.0 -0.672  0.451584  \n",
       "3                     0.167   21        0   0.0 -0.167  0.027889  \n",
       "4                     2.288   33        1   0.0 -2.288  5.234944  "
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "diabetes['diff'] = diabetes['pred']-diabetes['DiabetesPedigreeFunction'] \n",
    "diabetes['cuad'] = diabetes['diff']**2\n",
    "diabetes.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "1e45196b",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.727238Z",
     "iopub.status.busy": "2024-04-20T22:40:59.726761Z",
     "iopub.status.idle": "2024-04-20T22:40:59.735078Z",
     "shell.execute_reply": "2024-04-20T22:40:59.733875Z"
    },
    "papermill": {
     "duration": 0.024825,
     "end_time": "2024-04-20T22:40:59.737699",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.712874",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.33230294140625"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "diabetes['cuad'].mean()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "id": "3d11d618",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.763757Z",
     "iopub.status.busy": "2024-04-20T22:40:59.763254Z",
     "iopub.status.idle": "2024-04-20T22:40:59.774921Z",
     "shell.execute_reply": "2024-04-20T22:40:59.773979Z"
    },
    "papermill": {
     "duration": 0.028459,
     "end_time": "2024-04-20T22:40:59.777699",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.749240",
     "status": "completed"
    },
    "tags": []
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
       "      <th>w</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>50.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>53.061224</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>56.122449</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>59.183673</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>62.244898</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           w\n",
       "0  50.000000\n",
       "1  53.061224\n",
       "2  56.122449\n",
       "3  59.183673\n",
       "4  62.244898"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "w = np.linspace(50,200,50)\n",
    "grid_error = pd.DataFrame(w, columns = ['w'])\n",
    "grid_error.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "id": "f91c097f",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.804172Z",
     "iopub.status.busy": "2024-04-20T22:40:59.803421Z",
     "iopub.status.idle": "2024-04-20T22:40:59.810956Z",
     "shell.execute_reply": "2024-04-20T22:40:59.809644Z"
    },
    "papermill": {
     "duration": 0.024488,
     "end_time": "2024-04-20T22:40:59.814278",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.789790",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "def sum_error(w, diabetes):\n",
    "    b=0\n",
    "    diabetes['pred']= diabetes['Glucose']*w+b\n",
    "    diabetes['diff']= diabetes['pred']-diabetes['DiabetesPedigreeFunction']\n",
    "    diabetes['cuad']= diabetes['diff']**2\n",
    "    return(diabetes['cuad'].mean())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "id": "77a302b9",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.841324Z",
     "iopub.status.busy": "2024-04-20T22:40:59.840829Z",
     "iopub.status.idle": "2024-04-20T22:40:59.908245Z",
     "shell.execute_reply": "2024-04-20T22:40:59.906830Z"
    },
    "papermill": {
     "duration": 0.084787,
     "end_time": "2024-04-20T22:40:59.911597",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.826810",
     "status": "completed"
    },
    "tags": []
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
       "      <th>w</th>\n",
       "      <th>error</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>50.000000</td>\n",
       "      <td>3.908516e+07</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>53.061224</td>\n",
       "      <td>4.401799e+07</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>56.122449</td>\n",
       "      <td>4.924388e+07</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>59.183673</td>\n",
       "      <td>5.476282e+07</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>62.244898</td>\n",
       "      <td>6.057483e+07</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           w         error\n",
       "0  50.000000  3.908516e+07\n",
       "1  53.061224  4.401799e+07\n",
       "2  56.122449  4.924388e+07\n",
       "3  59.183673  5.476282e+07\n",
       "4  62.244898  6.057483e+07"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "grid_error['error']=grid_error['w'].apply(lambda x: sum_error(x, diabetes=diabetes))\n",
    "grid_error.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "id": "d6a3b209",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:40:59.941659Z",
     "iopub.status.busy": "2024-04-20T22:40:59.941188Z",
     "iopub.status.idle": "2024-04-20T22:41:00.248643Z",
     "shell.execute_reply": "2024-04-20T22:41:00.246490Z"
    },
    "papermill": {
     "duration": 0.327696,
     "end_time": "2024-04-20T22:41:00.252204",
     "exception": false,
     "start_time": "2024-04-20T22:40:59.924508",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "image/png": "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",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "grid_error.plot(x='w', y='error')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "f89420c2",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:41:00.280843Z",
     "iopub.status.busy": "2024-04-20T22:41:00.280324Z",
     "iopub.status.idle": "2024-04-20T22:41:00.716175Z",
     "shell.execute_reply": "2024-04-20T22:41:00.712214Z"
    },
    "papermill": {
     "duration": 0.457008,
     "end_time": "2024-04-20T22:41:00.722676",
     "exception": false,
     "start_time": "2024-04-20T22:41:00.265668",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "from sklearn.linear_model import LinearRegression"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "id": "ff5711e1",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:41:00.755368Z",
     "iopub.status.busy": "2024-04-20T22:41:00.754128Z",
     "iopub.status.idle": "2024-04-20T22:41:00.764397Z",
     "shell.execute_reply": "2024-04-20T22:41:00.761875Z"
    },
    "papermill": {
     "duration": 0.030405,
     "end_time": "2024-04-20T22:41:00.768525",
     "exception": false,
     "start_time": "2024-04-20T22:41:00.738120",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": [
    "x = np.array(diabetes['Glucose']).reshape((-1, 1))\n",
    "y= np.array(diabetes['DiabetesPedigreeFunction'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "2e1b9945",
   "metadata": {
    "papermill": {
     "duration": 0.019782,
     "end_time": "2024-04-20T22:41:00.806626",
     "exception": false,
     "start_time": "2024-04-20T22:41:00.786844",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "98ca88b9",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:41:00.845449Z",
     "iopub.status.busy": "2024-04-20T22:41:00.844952Z",
     "iopub.status.idle": "2024-04-20T22:41:00.867868Z",
     "shell.execute_reply": "2024-04-20T22:41:00.866619Z"
    },
    "papermill": {
     "duration": 0.045612,
     "end_time": "2024-04-20T22:41:00.870578",
     "exception": false,
     "start_time": "2024-04-20T22:41:00.824966",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<style>#sk-container-id-1 {color: black;background-color: white;}#sk-container-id-1 pre{padding: 0;}#sk-container-id-1 div.sk-toggleable {background-color: white;}#sk-container-id-1 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-1 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-1 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-1 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-1 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-1 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-1 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-1 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-1 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-1 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-1 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-1 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-1 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-1 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-1 div.sk-item {position: relative;z-index: 1;}#sk-container-id-1 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-1 div.sk-item::before, #sk-container-id-1 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-1 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-1 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-1 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-1 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-1 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-1 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-1 div.sk-label-container {text-align: center;}#sk-container-id-1 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-1 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-1\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>LinearRegression(fit_intercept=False)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-1\" type=\"checkbox\" checked><label for=\"sk-estimator-id-1\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">LinearRegression</label><div class=\"sk-toggleable__content\"><pre>LinearRegression(fit_intercept=False)</pre></div></div></div></div></div>"
      ],
      "text/plain": [
       "LinearRegression(fit_intercept=False)"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "model= LinearRegression(fit_intercept= False)\n",
    "model.fit(x,y)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "id": "2b52aefc",
   "metadata": {
    "execution": {
     "iopub.execute_input": "2024-04-20T22:41:00.899238Z",
     "iopub.status.busy": "2024-04-20T22:41:00.898726Z",
     "iopub.status.idle": "2024-04-20T22:41:00.905374Z",
     "shell.execute_reply": "2024-04-20T22:41:00.904165Z"
    },
    "papermill": {
     "duration": 0.025892,
     "end_time": "2024-04-20T22:41:00.909337",
     "exception": false,
     "start_time": "2024-04-20T22:41:00.883445",
     "status": "completed"
    },
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "intercepto(b): 0.0\n",
      "pendiente (w): [0.00374128]\n"
     ]
    }
   ],
   "source": [
    "print(f'intercepto(b): {model.intercept_}')\n",
    "print(f'pendiente (w): {model.coef_}')"
   ]
  }
 ],
 "metadata": {
  "kaggle": {
   "accelerator": "none",
   "dataSources": [
    {
     "datasetId": 2527538,
     "sourceId": 4289678,
     "sourceType": "datasetVersion"
    }
   ],
   "dockerImageVersionId": 30702,
   "isGpuEnabled": false,
   "isInternetEnabled": true,
   "language": "python",
   "sourceType": "notebook"
  },
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
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
   "version": "3.10.13"
  },
  "papermill": {
   "default_parameters": {},
   "duration": 9.570681,
   "end_time": "2024-04-20T22:41:01.848264",
   "environment_variables": {},
   "exception": null,
   "input_path": "__notebook__.ipynb",
   "output_path": "__notebook__.ipynb",
   "parameters": {},
   "start_time": "2024-04-20T22:40:52.277583",
   "version": "2.5.0"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
