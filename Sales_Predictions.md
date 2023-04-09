{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": [],
      "toc_visible": true,
      "mount_file_id": "1x5G_zgp6sASizJ7yZ757r_jgssDAnggG",
      "authorship_tag": "ABX9TyNO/hkA9pjTum0Hs3JMI2OK",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/BelandyG/Sales-Predictions/blob/main/Sales_Predictions.md\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Sales Predictions"
      ],
      "metadata": {
        "id": "JUsBZutY9W9s"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Belandy Gard"
      ],
      "metadata": {
        "id": "CN7vJj2p-zpN"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Loading Data"
      ],
      "metadata": {
        "id": "7-f5E8tT-GmI"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "from google.colab import drive\n",
        "drive.mount('/content/drive')"
      ],
      "metadata": {
        "id": "EmjHl_sv-NGC",
        "outputId": "1e8b17a5-7ccd-4373-8ddc-6fce15d18321",
        "colab": {
          "base_uri": "https://localhost:8080/"
        }
      },
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount(\"/content/drive\", force_remount=True).\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import pandas as pd \n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "import seaborn as sns\n",
        "          \n",
        "filename = ('/content/sales_predictions.csv')\n",
        "df = pd.read_csv(filename)\n",
        "df.head()"
      ],
      "metadata": {
        "id": "ZOLJJe11U1b-",
        "outputId": "0c8a686f-ff50-4409-ba16-13cc2296153b",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 351
        }
      },
      "execution_count": 4,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "  Item_Identifier  Item_Weight Item_Fat_Content  Item_Visibility  \\\n",
              "0           FDA15         9.30          Low Fat         0.016047   \n",
              "1           DRC01         5.92          Regular         0.019278   \n",
              "2           FDN15        17.50          Low Fat         0.016760   \n",
              "3           FDX07        19.20          Regular         0.000000   \n",
              "4           NCD19         8.93          Low Fat         0.000000   \n",
              "\n",
              "               Item_Type  Item_MRP Outlet_Identifier  \\\n",
              "0                  Dairy  249.8092            OUT049   \n",
              "1            Soft Drinks   48.2692            OUT018   \n",
              "2                   Meat  141.6180            OUT049   \n",
              "3  Fruits and Vegetables  182.0950            OUT010   \n",
              "4              Household   53.8614            OUT013   \n",
              "\n",
              "   Outlet_Establishment_Year Outlet_Size Outlet_Location_Type  \\\n",
              "0                       1999      Medium               Tier 1   \n",
              "1                       2009      Medium               Tier 3   \n",
              "2                       1999      Medium               Tier 1   \n",
              "3                       1998         NaN               Tier 3   \n",
              "4                       1987        High               Tier 3   \n",
              "\n",
              "         Outlet_Type  Item_Outlet_Sales  \n",
              "0  Supermarket Type1          3735.1380  \n",
              "1  Supermarket Type2           443.4228  \n",
              "2  Supermarket Type1          2097.2700  \n",
              "3      Grocery Store           732.3800  \n",
              "4  Supermarket Type1           994.7052  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-14f83d2d-2aca-454c-848b-59921e1d05e5\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Identifier</th>\n",
              "      <th>Item_Weight</th>\n",
              "      <th>Item_Fat_Content</th>\n",
              "      <th>Item_Visibility</th>\n",
              "      <th>Item_Type</th>\n",
              "      <th>Item_MRP</th>\n",
              "      <th>Outlet_Identifier</th>\n",
              "      <th>Outlet_Establishment_Year</th>\n",
              "      <th>Outlet_Size</th>\n",
              "      <th>Outlet_Location_Type</th>\n",
              "      <th>Outlet_Type</th>\n",
              "      <th>Item_Outlet_Sales</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>FDA15</td>\n",
              "      <td>9.30</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.016047</td>\n",
              "      <td>Dairy</td>\n",
              "      <td>249.8092</td>\n",
              "      <td>OUT049</td>\n",
              "      <td>1999</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 1</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "      <td>3735.1380</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>DRC01</td>\n",
              "      <td>5.92</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.019278</td>\n",
              "      <td>Soft Drinks</td>\n",
              "      <td>48.2692</td>\n",
              "      <td>OUT018</td>\n",
              "      <td>2009</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type2</td>\n",
              "      <td>443.4228</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>FDN15</td>\n",
              "      <td>17.50</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.016760</td>\n",
              "      <td>Meat</td>\n",
              "      <td>141.6180</td>\n",
              "      <td>OUT049</td>\n",
              "      <td>1999</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 1</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "      <td>2097.2700</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>FDX07</td>\n",
              "      <td>19.20</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.000000</td>\n",
              "      <td>Fruits and Vegetables</td>\n",
              "      <td>182.0950</td>\n",
              "      <td>OUT010</td>\n",
              "      <td>1998</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Grocery Store</td>\n",
              "      <td>732.3800</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>NCD19</td>\n",
              "      <td>8.93</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.000000</td>\n",
              "      <td>Household</td>\n",
              "      <td>53.8614</td>\n",
              "      <td>OUT013</td>\n",
              "      <td>1987</td>\n",
              "      <td>High</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "      <td>994.7052</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-14f83d2d-2aca-454c-848b-59921e1d05e5')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-14f83d2d-2aca-454c-848b-59921e1d05e5 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-14f83d2d-2aca-454c-848b-59921e1d05e5');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 4
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df2 = df.copy()"
      ],
      "metadata": {
        "id": "tvdfh4V5oDnr"
      },
      "execution_count": 5,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "df.info()"
      ],
      "metadata": {
        "id": "fHgx4EVvg0_0",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "694b86cd-26ce-4a12-869a-33b2874783b3"
      },
      "execution_count": 6,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 8523 entries, 0 to 8522\n",
            "Data columns (total 12 columns):\n",
            " #   Column                     Non-Null Count  Dtype  \n",
            "---  ------                     --------------  -----  \n",
            " 0   Item_Identifier            8523 non-null   object \n",
            " 1   Item_Weight                7060 non-null   float64\n",
            " 2   Item_Fat_Content           8523 non-null   object \n",
            " 3   Item_Visibility            8523 non-null   float64\n",
            " 4   Item_Type                  8523 non-null   object \n",
            " 5   Item_MRP                   8523 non-null   float64\n",
            " 6   Outlet_Identifier          8523 non-null   object \n",
            " 7   Outlet_Establishment_Year  8523 non-null   int64  \n",
            " 8   Outlet_Size                6113 non-null   object \n",
            " 9   Outlet_Location_Type       8523 non-null   object \n",
            " 10  Outlet_Type                8523 non-null   object \n",
            " 11  Item_Outlet_Sales          8523 non-null   float64\n",
            "dtypes: float64(4), int64(1), object(7)\n",
            "memory usage: 799.2+ KB\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Data Cleaning (Part 2)"
      ],
      "metadata": {
        "id": "NqP66KTm-Aqq"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#1) How many rows and columns?\n",
        "df.shape"
      ],
      "metadata": {
        "id": "p0bbchSL-rpg",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "d2bf2877-0e49-4290-fa2b-0a2ffe5eb400"
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(8523, 12)"
            ]
          },
          "metadata": {},
          "execution_count": 7
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "print(f\"There are {df.shape[0]} rows and {df.shape[1]} columns in this dataset.\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "CcNLjsFg3zlg",
        "outputId": "7037006f-3469-4a1b-d452-73165d3e6638"
      },
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "There are 8523 rows and 12 columns in this dataset.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#2) What are the datatypes of each variable?\n",
        "df.dtypes"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "UWlWa7nll6s1",
        "outputId": "87e94317-127e-4bcc-acd6-a319c4e6dcca"
      },
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Item_Identifier               object\n",
              "Item_Weight                  float64\n",
              "Item_Fat_Content              object\n",
              "Item_Visibility              float64\n",
              "Item_Type                     object\n",
              "Item_MRP                     float64\n",
              "Outlet_Identifier             object\n",
              "Outlet_Establishment_Year      int64\n",
              "Outlet_Size                   object\n",
              "Outlet_Location_Type          object\n",
              "Outlet_Type                   object\n",
              "Item_Outlet_Sales            float64\n",
              "dtype: object"
            ]
          },
          "metadata": {},
          "execution_count": 9
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#3) Are there duplicates? If so, drop any duplicates.\n",
        "df.duplicated().sum()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "l8rsmxfVoF0f",
        "outputId": "1fed1d00-3d60-4e61-cf22-5728cad7b272"
      },
      "execution_count": 10,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0"
            ]
          },
          "metadata": {},
          "execution_count": 10
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#4) I will Identify missing values.\n",
        "#There are 1463 missing valuses in Item_Weight\n",
        "#There are 2410 missing values in Outlet_Size\n",
        "sum_missing = df.isna().sum()\n",
        "df.isna().sum()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "bmf-oC-toXN6",
        "outputId": "cfed6e10-1521-4bb2-a932-240413ef5a15"
      },
      "execution_count": 11,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Item_Identifier                 0\n",
              "Item_Weight                  1463\n",
              "Item_Fat_Content                0\n",
              "Item_Visibility                 0\n",
              "Item_Type                       0\n",
              "Item_MRP                        0\n",
              "Outlet_Identifier               0\n",
              "Outlet_Establishment_Year       0\n",
              "Outlet_Size                  2410\n",
              "Outlet_Location_Type            0\n",
              "Outlet_Type                     0\n",
              "Item_Outlet_Sales               0\n",
              "dtype: int64"
            ]
          },
          "metadata": {},
          "execution_count": 11
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "missing_Item_Weight = sum_missing.loc['Item_Weight']\n",
        "percent_missing_Item_Weight = (((missing_Item_Weight) / (df.shape[0])*100)).round(2)\n",
        "print(f\"There are {missing_Item_Weight} missing values in the Item_Weight column, representing {percent_missing_Item_Weight}% of values missing.\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "R5LCBnaZ5vDw",
        "outputId": "9fb9a3ec-5d00-4220-b9d6-a4f0582bf515"
      },
      "execution_count": 12,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "There are 1463 missing values in the Item_Weight column, representing 17.17% of values missing.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# I will calculate percentage of missing values for Outlet_Size\n",
        "missing_Outlet_Size = sum_missing.loc['Outlet_Size']\n",
        "percent_missing_Outlet_Size = (((missing_Outlet_Size) / (df.shape[0])*100)).round(2)\n",
        "print(f\"There are {missing_Outlet_Size} missing values in the Item_Weight column, representing {percent_missing_Outlet_Size}% of values missing.\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "cWRyKSpo6Xnl",
        "outputId": "fc746cea-a9f1-4e6c-da1e-10f5665634d8"
      },
      "execution_count": 13,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "There are 2410 missing values in the Item_Weight column, representing 28.28% of values missing.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#5) Decide on how to address the missing values and do it! (This requires your judgement, so explain your choice).\n",
        "\n",
        "# I will not drop the missing values. Instead I will look to impute the missing values.\n",
        "\n",
        "# loop through all the rows in the df\n",
        "for index in df.index:\n",
        "\n",
        "  # create a filter for only items whose Item_Identifier matches that of the current row\n",
        "  item_identifier_filter = df['Item_Identifier'] == df.loc[index, 'Item_Identifier']\n",
        "\n",
        "  # calculate the mean item weight of the items in the filter\n",
        "  mean_item_weight = df.loc[item_identifier_filter, 'Item_Weight'].mean()\n",
        "\n",
        "  # assign the mean_item_weight to the Item_Weight column in this row\n",
        "  df.loc[index, 'Item_Weight'] = mean_item_weight\n"
      ],
      "metadata": {
        "id": "PCRzQvi6otrd"
      },
      "execution_count": 14,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# I will check if all Item_Weights have been imputed\n",
        "df.isna().sum()['Item_Weight']"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "shQFh_CV8xXL",
        "outputId": "a03007fe-061b-4b3e-e8ae-55bbb7aaa861"
      },
      "execution_count": 15,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "4"
            ]
          },
          "metadata": {},
          "execution_count": 15
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# I will check the 4 items left that are missing their Item_Weight\n",
        "filtered_df = df[df['Item_Weight'].isna()]\n",
        "filtered_df"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 303
        },
        "id": "NZVC-XTd8y7h",
        "outputId": "af7fd4ff-ed1c-4468-9bf1-d1712efe738d"
      },
      "execution_count": 16,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "     Item_Identifier  Item_Weight Item_Fat_Content  Item_Visibility  \\\n",
              "927            FDN52          NaN          Regular         0.130933   \n",
              "1922           FDK57          NaN          Low Fat         0.079904   \n",
              "4187           FDE52          NaN          Regular         0.029742   \n",
              "5022           FDQ60          NaN          Regular         0.191501   \n",
              "\n",
              "         Item_Type  Item_MRP Outlet_Identifier  Outlet_Establishment_Year  \\\n",
              "927   Frozen Foods   86.9198            OUT027                       1985   \n",
              "1922   Snack Foods  120.0440            OUT027                       1985   \n",
              "4187         Dairy   88.9514            OUT027                       1985   \n",
              "5022  Baking Goods  121.2098            OUT019                       1985   \n",
              "\n",
              "     Outlet_Size Outlet_Location_Type        Outlet_Type  Item_Outlet_Sales  \n",
              "927       Medium               Tier 3  Supermarket Type3          1569.9564  \n",
              "1922      Medium               Tier 3  Supermarket Type3          4434.2280  \n",
              "4187      Medium               Tier 3  Supermarket Type3          3453.5046  \n",
              "5022       Small               Tier 1      Grocery Store           120.5098  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-064bbd6b-c941-448c-9765-1f971a752d3a\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Identifier</th>\n",
              "      <th>Item_Weight</th>\n",
              "      <th>Item_Fat_Content</th>\n",
              "      <th>Item_Visibility</th>\n",
              "      <th>Item_Type</th>\n",
              "      <th>Item_MRP</th>\n",
              "      <th>Outlet_Identifier</th>\n",
              "      <th>Outlet_Establishment_Year</th>\n",
              "      <th>Outlet_Size</th>\n",
              "      <th>Outlet_Location_Type</th>\n",
              "      <th>Outlet_Type</th>\n",
              "      <th>Item_Outlet_Sales</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>927</th>\n",
              "      <td>FDN52</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.130933</td>\n",
              "      <td>Frozen Foods</td>\n",
              "      <td>86.9198</td>\n",
              "      <td>OUT027</td>\n",
              "      <td>1985</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type3</td>\n",
              "      <td>1569.9564</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1922</th>\n",
              "      <td>FDK57</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.079904</td>\n",
              "      <td>Snack Foods</td>\n",
              "      <td>120.0440</td>\n",
              "      <td>OUT027</td>\n",
              "      <td>1985</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type3</td>\n",
              "      <td>4434.2280</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4187</th>\n",
              "      <td>FDE52</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.029742</td>\n",
              "      <td>Dairy</td>\n",
              "      <td>88.9514</td>\n",
              "      <td>OUT027</td>\n",
              "      <td>1985</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type3</td>\n",
              "      <td>3453.5046</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5022</th>\n",
              "      <td>FDQ60</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.191501</td>\n",
              "      <td>Baking Goods</td>\n",
              "      <td>121.2098</td>\n",
              "      <td>OUT019</td>\n",
              "      <td>1985</td>\n",
              "      <td>Small</td>\n",
              "      <td>Tier 1</td>\n",
              "      <td>Grocery Store</td>\n",
              "      <td>120.5098</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-064bbd6b-c941-448c-9765-1f971a752d3a')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-064bbd6b-c941-448c-9765-1f971a752d3a button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-064bbd6b-c941-448c-9765-1f971a752d3a');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 16
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# I confirm that these Item_Identifiers each only appeared in the dataset once\n",
        "for index, row in filtered_df.iterrows():\n",
        "  identifier = row['Item_Identifier']\n",
        "  number = df['Item_Identifier'].value_counts()[identifier]\n",
        "  print(f\"The Item_Idenfier {identifier} appears in the dataset {number} time(s).\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "6noH8Vqq84Zp",
        "outputId": "6232cc89-587a-4ad4-efc5-21798e7ff732"
      },
      "execution_count": 17,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "The Item_Idenfier FDN52 appears in the dataset 1 time(s).\n",
            "The Item_Idenfier FDK57 appears in the dataset 1 time(s).\n",
            "The Item_Idenfier FDE52 appears in the dataset 1 time(s).\n",
            "The Item_Idenfier FDQ60 appears in the dataset 1 time(s).\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# since they are all unique items, their Item_Weight is unknown\n",
        "# set their Item_Weights to the average Item_Weight\n",
        "\n",
        "for index, row in filtered_df.iterrows():\n",
        "  df.loc[index, 'Item_Weight'] = df['Item_Weight'].mean()"
      ],
      "metadata": {
        "id": "-FCbhBAmpXf9"
      },
      "execution_count": 18,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# check if all Item_Weights have been imputed\n",
        "df.isna().sum()['Item_Weight']"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "jFQxxYtFp3vU",
        "outputId": "09ed1bc4-46fc-4ad9-b696-dbba434d5b16"
      },
      "execution_count": 19,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0"
            ]
          },
          "metadata": {},
          "execution_count": 19
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# loop through all the rows in the df\n",
        "for index in df.index:\n",
        "\n",
        "  # create a filter for only items whose Item_Identifier matches that of the current row\n",
        "  outlet_identifier_filter = df['Outlet_Identifier'] == df.loc[index, 'Outlet_Identifier']\n",
        "\n",
        "  # calculate the mean item weight of the items in the filter\n",
        "  outlet_size = df.loc[outlet_identifier_filter, 'Outlet_Size'].min()\n",
        "\n",
        "  # assign the mean_item_weight to the Item_Weight column in this row\n",
        "  df.loc[index, 'Outlet_Size'] = outlet_size\n",
        "\n"
      ],
      "metadata": {
        "id": "iiqP1hq-YPlC"
      },
      "execution_count": 20,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# check if all Outlet_Sizes have been imputed\n",
        "df.isna().sum()['Outlet_Size']"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "VtrgspmqrS6N",
        "outputId": "9c25dd4c-838d-41ff-ceaf-6ab1d029991c"
      },
      "execution_count": 21,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "2410"
            ]
          },
          "metadata": {},
          "execution_count": 21
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# none of the Outlet_Identifiers missing their Outlet_Size had an Outlet_Size in another row\n",
        "# since they are all unique outlets, their Outlet_Size is unknown\n",
        "# set their Outlet_Sizes to 'Unknown'\n",
        "\n",
        "df['Outlet_Size'].fillna('Unknown', inplace = True)"
      ],
      "metadata": {
        "id": "mOtmISZFr420"
      },
      "execution_count": 22,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# I will check if all Outlet_Sizes have been imputed\n",
        "df.isna().sum()['Outlet_Size']"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "u5nf9jKHtKz0",
        "outputId": "502a1fa3-64fc-4fb7-bef9-6b75880c8cd8"
      },
      "execution_count": 23,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0"
            ]
          },
          "metadata": {},
          "execution_count": 23
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "missing = df.isna().sum().sum()\n",
        "print(f\"There are {missing} missing values remaining.\")"
      ],
      "metadata": {
        "id": "hJyRlZWWbdVT",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "185d59a8-99a9-47af-c6a3-312971cc6861"
      },
      "execution_count": 24,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "There are 0 missing values remaining.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#6) I Confirm that there are no missing values after addressing them.\n",
        "df.isna().sum()\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "0n9KMBapKNKw",
        "outputId": "cb14625d-8624-431b-a5bd-3387d1f3f1e2"
      },
      "execution_count": 25,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Item_Identifier              0\n",
              "Item_Weight                  0\n",
              "Item_Fat_Content             0\n",
              "Item_Visibility              0\n",
              "Item_Type                    0\n",
              "Item_MRP                     0\n",
              "Outlet_Identifier            0\n",
              "Outlet_Establishment_Year    0\n",
              "Outlet_Size                  0\n",
              "Outlet_Location_Type         0\n",
              "Outlet_Type                  0\n",
              "Item_Outlet_Sales            0\n",
              "dtype: int64"
            ]
          },
          "metadata": {},
          "execution_count": 25
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#7) Find and fix any inconsistent categories of data (example: fix cat, Cat, and cats so that they are consistent).\n",
        "df.describe(include = 'object').round(2)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 172
        },
        "id": "2_tWADu-MS2T",
        "outputId": "3ec79c82-112d-4cd3-d517-4643de13c37f"
      },
      "execution_count": 26,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "       Item_Identifier Item_Fat_Content              Item_Type  \\\n",
              "count             8523             8523                   8523   \n",
              "unique            1559                5                     16   \n",
              "top              FDW13          Low Fat  Fruits and Vegetables   \n",
              "freq                10             5089                   1232   \n",
              "\n",
              "       Outlet_Identifier Outlet_Size Outlet_Location_Type        Outlet_Type  \n",
              "count               8523        8523                 8523               8523  \n",
              "unique                10           4                    3                  4  \n",
              "top               OUT027      Medium               Tier 3  Supermarket Type1  \n",
              "freq                 935        2793                 3350               5577  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-1d5540d7-9632-4f79-8af2-753437f3a2ab\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Identifier</th>\n",
              "      <th>Item_Fat_Content</th>\n",
              "      <th>Item_Type</th>\n",
              "      <th>Outlet_Identifier</th>\n",
              "      <th>Outlet_Size</th>\n",
              "      <th>Outlet_Location_Type</th>\n",
              "      <th>Outlet_Type</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>unique</th>\n",
              "      <td>1559</td>\n",
              "      <td>5</td>\n",
              "      <td>16</td>\n",
              "      <td>10</td>\n",
              "      <td>4</td>\n",
              "      <td>3</td>\n",
              "      <td>4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>top</th>\n",
              "      <td>FDW13</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>Fruits and Vegetables</td>\n",
              "      <td>OUT027</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>freq</th>\n",
              "      <td>10</td>\n",
              "      <td>5089</td>\n",
              "      <td>1232</td>\n",
              "      <td>935</td>\n",
              "      <td>2793</td>\n",
              "      <td>3350</td>\n",
              "      <td>5577</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-1d5540d7-9632-4f79-8af2-753437f3a2ab')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-1d5540d7-9632-4f79-8af2-753437f3a2ab button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-1d5540d7-9632-4f79-8af2-753437f3a2ab');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 26
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#8)For any numerical columns, obtain the summary statistics of each (min, max, mean).\n",
        "df.describe(include = 'number').round(2)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 295
        },
        "id": "B65lacxCQavO",
        "outputId": "47f80223-0948-45e2-ae5c-ad8cefa80bce"
      },
      "execution_count": 27,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "       Item_Weight  Item_Visibility  Item_MRP  Outlet_Establishment_Year  \\\n",
              "count      8523.00          8523.00   8523.00                    8523.00   \n",
              "mean         12.88             0.07    140.99                    1997.83   \n",
              "std           4.65             0.05     62.28                       8.37   \n",
              "min           4.56             0.00     31.29                    1985.00   \n",
              "25%           8.78             0.03     93.83                    1987.00   \n",
              "50%          12.65             0.05    143.01                    1999.00   \n",
              "75%          16.85             0.09    185.64                    2004.00   \n",
              "max          21.35             0.33    266.89                    2009.00   \n",
              "\n",
              "       Item_Outlet_Sales  \n",
              "count            8523.00  \n",
              "mean             2181.29  \n",
              "std              1706.50  \n",
              "min                33.29  \n",
              "25%               834.25  \n",
              "50%              1794.33  \n",
              "75%              3101.30  \n",
              "max             13086.96  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-c4599663-dde7-4c19-a5df-d7c7f61a8dc6\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Weight</th>\n",
              "      <th>Item_Visibility</th>\n",
              "      <th>Item_MRP</th>\n",
              "      <th>Outlet_Establishment_Year</th>\n",
              "      <th>Item_Outlet_Sales</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>8523.00</td>\n",
              "      <td>8523.00</td>\n",
              "      <td>8523.00</td>\n",
              "      <td>8523.00</td>\n",
              "      <td>8523.00</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>12.88</td>\n",
              "      <td>0.07</td>\n",
              "      <td>140.99</td>\n",
              "      <td>1997.83</td>\n",
              "      <td>2181.29</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>4.65</td>\n",
              "      <td>0.05</td>\n",
              "      <td>62.28</td>\n",
              "      <td>8.37</td>\n",
              "      <td>1706.50</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>4.56</td>\n",
              "      <td>0.00</td>\n",
              "      <td>31.29</td>\n",
              "      <td>1985.00</td>\n",
              "      <td>33.29</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>8.78</td>\n",
              "      <td>0.03</td>\n",
              "      <td>93.83</td>\n",
              "      <td>1987.00</td>\n",
              "      <td>834.25</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>12.65</td>\n",
              "      <td>0.05</td>\n",
              "      <td>143.01</td>\n",
              "      <td>1999.00</td>\n",
              "      <td>1794.33</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>16.85</td>\n",
              "      <td>0.09</td>\n",
              "      <td>185.64</td>\n",
              "      <td>2004.00</td>\n",
              "      <td>3101.30</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>21.35</td>\n",
              "      <td>0.33</td>\n",
              "      <td>266.89</td>\n",
              "      <td>2009.00</td>\n",
              "      <td>13086.96</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-c4599663-dde7-4c19-a5df-d7c7f61a8dc6')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-c4599663-dde7-4c19-a5df-d7c7f61a8dc6 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-c4599663-dde7-4c19-a5df-d7c7f61a8dc6');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 27
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Exploratory Visual (Part 3)"
      ],
      "metadata": {
        "id": "k18pGl25-Vos"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df.info()"
      ],
      "metadata": {
        "id": "SN6gxpzv-oLa",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "f87b7fae-d582-419d-ffce-351c5600608b"
      },
      "execution_count": 28,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 8523 entries, 0 to 8522\n",
            "Data columns (total 12 columns):\n",
            " #   Column                     Non-Null Count  Dtype  \n",
            "---  ------                     --------------  -----  \n",
            " 0   Item_Identifier            8523 non-null   object \n",
            " 1   Item_Weight                8523 non-null   float64\n",
            " 2   Item_Fat_Content           8523 non-null   object \n",
            " 3   Item_Visibility            8523 non-null   float64\n",
            " 4   Item_Type                  8523 non-null   object \n",
            " 5   Item_MRP                   8523 non-null   float64\n",
            " 6   Outlet_Identifier          8523 non-null   object \n",
            " 7   Outlet_Establishment_Year  8523 non-null   int64  \n",
            " 8   Outlet_Size                8523 non-null   object \n",
            " 9   Outlet_Location_Type       8523 non-null   object \n",
            " 10  Outlet_Type                8523 non-null   object \n",
            " 11  Item_Outlet_Sales          8523 non-null   float64\n",
            "dtypes: float64(4), int64(1), object(7)\n",
            "memory usage: 799.2+ KB\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "num_cols = df.select_dtypes('number').columns\n",
        "num_cols    "
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Rhz-VYkj_Pso",
        "outputId": "2d514c9f-5e4f-43e5-88c0-f952768ada55"
      },
      "execution_count": 29,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Index(['Item_Weight', 'Item_Visibility', 'Item_MRP',\n",
              "       'Outlet_Establishment_Year', 'Item_Outlet_Sales'],\n",
              "      dtype='object')"
            ]
          },
          "metadata": {},
          "execution_count": 29
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df['Outlet_Location_Type'].value_counts()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "KoWwjYrG_X-9",
        "outputId": "48570816-7636-4a71-c2f0-2d4c150ec998"
      },
      "execution_count": 30,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Tier 3    3350\n",
              "Tier 2    2785\n",
              "Tier 1    2388\n",
              "Name: Outlet_Location_Type, dtype: int64"
            ]
          },
          "metadata": {},
          "execution_count": 30
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# This histogram shows the relationshiop of Item_Outlet_Sales and Outlet location. This can give a better understanding of which outlet produces the best sales.\n",
        "ax = df['Item_Outlet_Sales'].hist(bins = 30, edgecolor = 'black')\n",
        "ax.tick_params(axis='x', rotation = 45)\n",
        "#ax.ticklabel_format(style='plain')\n",
        "ax.set_title('Distribution of Item Sales')\n",
        "ax.set_xlabel('Item_Outlet_Sales')\n",
        "ax.set_ylabel('Outlet_Location_Type')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 516
        },
        "id": "P6ttgyFQD7hP",
        "outputId": "631cf91c-841b-4a9e-9661-4104eb76fa47"
      },
      "execution_count": 31,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Text(0, 0.5, 'Outlet_Location_Type')"
            ]
          },
          "metadata": {},
          "execution_count": 31
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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QERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyZ9BAdPLkSbz77rtwc3ODQqHA3r17NdYLITBv3jyUK1cOFhYWCAgIwI0bNzS2SUhIwIABA2Brawt7e3sMGzYMycnJGttcuXIF77zzDszNzeHh4YGVK1cW9qERERFRCWLQQJSSkoK6deti48aNua5fuXIl1q1bh02bNuHcuXOwsrJCYGAg0tPTpW0GDBiAyMhIhIaGYv/+/Th58iRGjhwprU9KSkL79u3h5eWF8PBwfPzxx1iwYAG++OKLQj8+IiIiKhmMDfnmHTt2RMeOHXNdJ4TAmjVrMGfOHHTr1g0AsGPHDri4uGDv3r3o27cvoqKicOjQIVy4cAG+vr4AgPXr16NTp05YtWoV3NzcsHPnTmRmZuLrr7+GqakpfHx8EBERgU8//VQjOBEREZF8GTQQvU5MTAxiY2MREBAgLbOzs0OTJk0QFhaGvn37IiwsDPb29lIYAoCAgAAolUqcO3cOPXr0QFhYGFq2bAlTU1Npm8DAQKxYsQLPnj1DmTJlcn3/jIwMZGRkSK+TkpIAACqVCiqVSi/HmLMfCwsLmBsrYGokCjQ+y8RI57EKYwUsLCygVqv1djz6kFNLcarJ0NgTbeyJNvZEG3uiTY49ye+xFttAFBsbCwBwcXHRWO7i4iKti42NhbOzs8Z6Y2NjODg4aGzj7e2ttY+cdXkFomXLlmHhwoVay0NCQmBpaanDEeXt66+//r8/ZRdsYONmwJBmuo2FF/Dubjx48AAPHjwo4NjCFxoaaugSih32RBt7oo090caeaJNTT1JTU/O1XbENRIY2c+ZMTJ48WXqdlJQEDw8PtG/fHra2tnp5D5VKhdDQUAwdOhS2PebD1KVigcanRP2JhEPr4dJ/eYHHZsbdRtyuGTh58iTq1q1boLGFKacn7dq1g4mJiaHLKRbYE23siTb2RBt7ok2OPcm5wvMmxTYQubq6AgDi4uJQrlw5aXlcXBzq1asnbRMfH68xLisrCwkJCdJ4V1dXxMXFaWyT8zpnm9yYmZnBzMxMa7mJiYne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NjYWc+bMwZgxY3I9C0RERETyUuwD0eeffw4A8Pf311i+detWfPDBBwCA1atXQ6lUomfPnsjIyEBgYCA+++wzaVsjIyPs378fo0aNgp+fH6ysrDBkyBAEBwcX1WEQERFRMVbsA1F+boIzNzfHxo0bsXHjxjy38fLywoEDB/RZGhEREZUSxT4QEZFhREVF6TTOyckJnp6eeq6GiKhwMRARkYbs5GeAQoGBAwfqNN7cwhLR16MYioioRGEgIiIN6oxkQAg4dpkCE0ePAo1VPb2Pp/s/wZMnTxiIiKhEYSAiolyZOHrAzLWyocsgIioSDEREpHecf0REJQ0DERHpDecfEVFJxUBERHrD+UdEVFIxEBGR3nH+ERGVNMX+s8yIiIiIChsDEREREckeAxERERHJHgMRERERyR4DEREREcke7zIjomIlPw91VKvVAIDLly9Dqfz//67jgx2JSFcMRERULBTkoY4WFhbYvXs3WrZsibS0NGk5H+xIRLpiICKiYqEgD3U0N1YAAFz6L0d6lgDABzsS0dthICKiYiU/D3U0NRIAsmHqUhEiW1E0hRFRqcZJ1URERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7/LR7IipVoqKidBrn5OQET09PPVdDRCUFAxERlQrZyc8AhQIDBw7Uaby5hSWir0cxFBHJFAMREZUK6oxkQAg4dpkCE0ePAo1VPb2Pp/s/wZMnTxiIiGSKgYiIShUTRw+YuVY2dBlEVMJwUjURERHJHs8QERH9H07IJpIvBiIikj1OyCYiBiIikj1OyCYiBiIiov/DCdlE8sVARESkB5x/RFSyMRAREb0Fzj8iKh0YiIiI3gLnHxGVDgxERER6wPlHRCUbH8xIREREssczREREBsYJ2USGx0BERGQgnJBNVHwwEBERGYg+JmT/+eefqFGjBtRqNQDg8uXLUCrfPBuCZ5eINDEQEREZmC4Tsl89u2RhYYHdu3ejZcuWSEtLe+N4nl0i0iSrQLRx40Z8/PHHiI2NRd26dbF+/Xo0btzY0GURERXYq2eXzI0VAACX/suRniVeO5a3+xNpk00g2rNnDyZPnoxNmzahSZMmWLNmDQIDAxEdHQ1nZ2dD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4+LFi0hLS8PYsWNx/Phx3L17F6dPn8aFCxdQo0aNAv0/ICrtGIiIKE9Tp06FkZERatasibJly+LevXtwc3PD6dOnkZ2djfbt26N27dqYOHEi7O3toVQW3q+UNWvW4LPPPsPu3btRq1Yt+Pr64uTJk9i7d6/GWaMaNWrgs88+w8aNG1G3bl2cP38eU6dO1djXiBEjUK1aNfj6+qJs2bI4ffq01vvl5zhz68/rtGrVCrdv38bgwYNRvXp1dOzYEbGxsQgJCUG1atUAAJ9++inKlCmDZs2a4d1330VgYCAaNGiQ5z4VCgUOHDiAli1bIigoCFWrVkXfvn1x9+5duLi4wMjICE+fPsXgwYNRtWpV9O7dGx07dsTChQvz3XsiOVCIop6pSERERFTM8AwRERERyR4DEREZ3Icffqhxy/h/vz788ENDl5dvpeU4iOSIl8yIyODi4+ORlJSU6zpbW1s4OzsXcUW6KS3HQSRHDEREREQke7xkRkRERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESy9/8AzrKiDKCFT0kAAAAASUVORK5CYII=\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "ax = sns.histplot(data = df, y = 'Item_Type', x='Outlet_Location_Type')\n",
        "ax.tick_params(axis='x', rotation = 45)\n",
        "ax.set_ylabel('Outlet_Location_Type')\n",
        "ax.set_title('Distribution of Fat Content')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 513
        },
        "id": "QlylCTqSlE1E",
        "outputId": "b30ae8a8-2caf-4722-aaf4-5bcc01e53b62"
      },
      "execution_count": 32,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Text(0.5, 1.0, 'Distribution of Fat Content')"
            ]
          },
          "metadata": {},
          "execution_count": 32
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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MnnNXPz9/dGxY0fMnj0bw4YNe2aP9cKFC3Hw4EFcunQJYWFhqF+/vnz94NmzZ+PEiROYPHkyUlNTce3aNSQmJnJCGdUOFh7zS0REVZSdnS1GjRolXF1dha2trfDw8BARERHip59+ktvk5uaKXr16ibp164rmzZuLPXv2mEwoE0KImJgY4eHhIVQqlejSpYsQouJkL4PBIFavXi38/PyEra2taNCggQgJCRFHjhyR2yxcuFC4ubkJSZLEqFGjXhr/qFGjBIAKjzFjxgghhDh8+LBo166d0Gg0ws3NTcyePVs8efJEfn15ebmIiooSnp6ewtbWVjRt2lR8+umn8vpZs2YJFxcXodfrxZAhQ8SqVauEo6OjvL6kpEQMHjxYODk5CQDyMcFTk/AuXLggunXrJuzs7ES9evXEuHHjRGFhocl+PD0xburUqfKxrKznTXDbtGmTACCSk5NNlhsnlP3jH/8QrVq1EhqNRrRv316kpaWZtEtOThbBwcFCr9eLunXrirfeekssXry4SrERWSNJiGqamUBERESVtmjRIiQkJODChQsmyw8fPoxu3brhwYMHcHJyskxwRDUYhyUQERHVIEVFRbh06RKio6MRERFh6XCIrA6LWyIiemNu3rxpcvmppx9VvSSXtTp27NgLj8OLTJ48GW3atEHXrl3xwQcfVFPERMrBYQlERPTGlJWVITs7+7nrvby8YGOj/Av1FBcXIzc397nreRUDIvNhcUtEREREisFhCURERESkGCxuiYiIiEgxWNwSERERkWKwuCUiIiIixWBxS0RERESKweKWiIiIiBSDxS0RERERKQaLWyIiIiJSjP8CcjySfa40ODoAAAAASUVORK5CYII=\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "ax = sns.boxplot(x='Item_Weight', y='Item_Type', data=df)\n",
        "ax.set_title(\"Comparison of Product Type and Weight\");"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 472
        },
        "id": "G-5fhgzwjnRl",
        "outputId": "9dfe23c2-5f9d-4bd6-a1ba-a366204a195f"
      },
      "execution_count": 33,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import seaborn as sns\n",
        "corr = df.corr()\n",
        "sns.heatmap(corr, cmap = 'Purples', annot = True)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 623
        },
        "id": "JbpN5aBMqQWU",
        "outputId": "6edab69e-1355-4ab1-deeb-b791ea84379f"
      },
      "execution_count": 34,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "<Axes: >"
            ]
          },
          "metadata": {},
          "execution_count": 34
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 2 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAq4AAAJNCAYAAADwL/cqAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/bCgiHAAAACXBIWXMAAA9hAAAPYQGoP6dpAACt10lEQVR4nOzdd1QUVxsG8GfpXWmCIEWlKkVsCDZQrBHFJBZUFFTsFSv2FlGjBowFS1Qs2EtMjD1ivhgjlihqFKMJYgGERVQUqfv9QVxdWRQQXCY8P8+cIzN37tw73F3evfPOrEgikUhARERERFTJKSm6AUREREREJcHAlYiIiIgEgYErEREREQkCA1ciIiIiEgQGrkREREQkCAxciYiIiEgQGLgSERERkSAwcCUiIiIiQWDgSkRERESCwMCViIiIiASBgSsRERERlcovv/wCX19fmJmZQSQS4eDBgx/cJyYmBg0bNoS6ujpsbGywefPmUh+XgSsRERERlcqLFy/g6uqKVatWlaj8P//8g88++wze3t64cuUKxo0bh8GDB+PYsWOlOq5IIpFIytJgIiIiIiKRSIQDBw7Az8+v2DJTpkzB4cOHcf36dem63r17IyMjA0ePHi3xsTjjSkRERETIzs7Gs2fPZJbs7OxyqfvcuXPw8fGRWdehQwecO3euVPWolEtriKoQL9EsRTdBsA49CVV0EwQtL69A0U0QLBVVztOUVUE+L8x+jOoGWhVaf3n+TfKarYS5c+fKrJs9ezbmzJnz0XUnJyfDxMREZp2JiQmePXuGrKwsaGpqlqgeBq5EREREAiUSicqtrtDQUISEhMisU1dXL7f6ywMDVyIiIiKhKr+4Ferq6hUWqJqamiIlJUVmXUpKCvT09Eo82wowx5WIiIiIKpiHhwdOnTols+7EiRPw8PAoVT0MXImIiIgESqQkKrelNDIzM3HlyhVcuXIFQOHjrq5cuYLExEQAhWkH/fv3l5YfNmwY/v77b0yePBm3bt3C6tWrsXv3bowfP75Ux2WqABEREZFAlWOKa6lcvHgR3t7e0p9f58YOGDAAmzdvRlJSkjSIBYDatWvj8OHDGD9+PCIiIlCrVi1s2LABHTp0KNVxGbgSERERUal4eXnhfV8FIO9bsby8vPDHH3981HEZuBIREREJlaKmXBWEgSsRERGRQFWxuJU3ZxERERGRMHDGlYiIiEigSvs0AKFj4EpEREQkVFUsV4CpAkREREQkCJxxJSIiIhKoKjbhysCViIiISKhEVSxyZeBKREREJFRVK25ljisRERERCQNnXImIiIgEio/DIiIiIiJBqGIprkwVICIiIiJh4IwrERERkVBVsSlXBq5EREREAlXF4lamChARERGRMHDGlYiIiEig+FQBIiIiIhKGKpYrwFQBIiIiIhIEzrgSERERCVQVm3Bl4EpEREQkVKIqFrkycCUiIiISqqoVtzLHlYiIiIiEgTOuRERERALFx2ERVTJeXl5o0KABwsPDS7yPSCTCgQMH4OfnV2HtqkxcWlqh96QWsGtUE0ZmepjhF41fv7+l6GZVKIlEgrXr1uDg9/uRmfkcLi4NMHXyNFhaWr13v917dmLb9iiIxWLY2tph0oQpqF/fWbo9Ozsb4RHLcOLEMeTk5qCZuyemTJ4GQ0NDaZnYC+cRuXYV7t69Aw0NTXT5zBfDh42CikrhW+qlSxcQvWM7bvx5HS9eZMLCwhIB/QagU8fPKuZkfKS9+3Zhe/QWpKeLYWNjh5Dxk1G/nlOx5U/9fALr1q9BcvIj1KpliZHDx8DTswUAIC8vF2vXrcZv587i0aMH0NHWQeMm7hgxbAyMjY1l6jn72/+wcdN63LnzF9TV1eDWoBEWL1peoX2tCNKxePCtsTilhGNx21tjcaLsWNx/YC+OHTuC+PhbePHiBX4+9Qt0dfVk6giZMBa3b8fjyZN06OrqoWlTd4weNRbGxjUqpK/lbc/eXdi+PQridDFsbewwIWQK6td/z9g7dQJr161GUvIjWNSyxMiRY9Dcs6V0++mYU9h/YC9u3bqJZ8+eYmvUTtjZ2cvUEbZoAS5cPI+01FRoamnC2dkVo0aMhbV17QrrZ4WqWnErUwWEKjAwUBqUeXl5Ydy4cQptDwDUrFkTixYtklk3depUiEQixMTEyKz38vJCQEBAierdv38/5s+fX17NBADExMRAJBIhIyOjXOtVFA1tNdy9mozwkYcV3ZRPZsvWzdi1OxqhU6Zj03dboamhidFjRyA7O7vYfY6fOIbwiGUYPGgotkbtgK2NHUaPHYH09HRpmW/Cl+J/v/6CsLCvsXbNd0hLS8XkqSHS7bdvx2Pc+FHwaNYc27bsxMKvFuOX/53BylUrpGXirl2FjY0tFi9aih3b98C3SzfMmTsT//v1l4o5GR/h5MljWPHtcgwaOASbN0bD1sYW40NGIv1JutzycdeuYvacafDt0g1Rm6LRqqUXpoSG4O7fdwAAr169Qnz8LQQFDsbmjdEIW7gUiYn3MHnKOJl6Tp8+hbnzZuKzzl2xNWon1q7ZhHbtOlZ0dyvEli2bsWtXNEKnTsemjVuhqamJ0WNKMBbDl2Hw4KHYumUHbG3tMHqM7Fh89eoVPDyaIzBwULH1NG7UGGELl2DvnoNYvHgpHjy4jylTJ5Zr/yrKiZPHELFiGQYNGoqozdGwsbXD2PGy5+BtcXFXMHN2KHx9/bAlagdatfLC5CkhuHv3jrRMVlYWXF0aYNTIMcUe18HBETOnz8HOnfsREb4akEgwZtwI5Ofnl3sfqfwxcKVy4+XlVSRAPX36NCwsLGTWv3r1Cr///jvatGlTonoNDAygq6tbji3974k9+he+m3kKvx68qeimfBISiQQ7dm7HwKBgtG7tDVtbO8ydMx9paak4c+Z0sftF79gKv26fo6uvH+rUqYvQqTOgoaGBQz8cBABkZj7H94cOYPzYCWjSuCkcHeth1sy5iIu7imvX4gAU/rG1sbFF8OChsLCwRKOGjTF61Djs3bcLL168AAAEBQ7G8GEj4erSALVqWcC/d194NPPE6dOnKvzclNaOXdvR1bc7unzWDbVr18HkSdOhrq6BH3/8Xm753buj4e7ugX59B8Daug6GDhkBezsH7N27CwCgo6OLFRFr4NO2PaysrOHk5IIJIVNwK/4mkpOTAAB5eXn4JuJrjBo5Dp93/xKWllaoXbsOfNq2/2T9Li/SsTiwlGMxeiv8/IofiwDQx78fAgcMhLOTc7H19OkTAGdnF9SsaQZXlwYYMGAgrl+/hry83PLsZoXYsWMbunX9HL5duqFO7bqYOnk6NNQ18MOPB+WW37V7B5q5eyKg3wDUtq6DYUNHwt7eEXv27pSW6dypCwYPGoomTZoVe9zufl/Aza0RzGqawcHeEUOHjkRKSjKSkh6Vdxc/CZFIVG6LEDBwFbjAwECcOXMGERER0oGXkJAAALh+/To6deoEHR0dmJiYICAgAGlpadJ9vby8MHr0aIwbNw76+vowMTHB+vXr8eLFCwQFBUFXVxc2NjY4cuRIidri7e2Ns2fPIi8vDwDw/Plz/PHHH5gyZYpM4Hru3DlkZ2fD29u7xO18e0Y5KSkJn332GTQ1NVG7dm1ER0fD2tq6SCpBWloaunfvDi0tLdja2uLQoUMAgISEBOmx9fX1IRKJEBgYWKI+UuXw8NFDiMVpaNrUXbpOR0cX9es7I+7aVbn75Obm4tatmzL7KCkpoWkTd2lQevPWTeTl5cmUsbauDVPTmrh2vbDenNxcqKupy9Strq6O7Oxs3Lr1Z7FtzszMhJ5etdJ3tgLl5uYiPv4mmjSRPSdNGrvj+vU4uftcv3ENTRq7y6xzd/fA9RvyywOFfReJRNIPoPG3byE19TGUlEToH+iPLl3bY/yEUdJZWyH5qLHYpPixWBZPnz7F0aM/wcXFFSoqqmWu51PIzc3Frfii56BJE3dcK2bsXbseJzNWAaCZu0ex5UsiKysLP/54CGZm5jAxMS1zPYrEwJUEJSIiAh4eHggODkZSUhKSkpJgYWGBjIwMtGnTBm5ubrh48SKOHj2KlJQU9OzZU2b/qKgoGBkZITY2FqNHj8bw4cPRo0cPeHp64vLly2jfvj0CAgLw8uXLD7bF29sbmZmZuHDhAgDgf//7H+zs7PDFF1/g/PnzePXqFYDCWVhra2tYW1uXuJ1v69+/Px49eoSYmBjs27cP69atw+PHj4uUmzt3Lnr27Im4uDh07twZffv2RXp6OiwsLLBv3z4AQHx8PJKSkhAREVHic06KJxYXfrAxNDCUWW9oYABxuljuPhkZT5Cfnw+Dd/YxMDCEOD1NWq+qqmqRPEIDAwOIxYX1erh7IO7aVRw7dgT5+fl4/DgF3323DgBkPnC97cTJY/jz5g34+nYtZU8rVkZGxr/nxEBmvcF7zqNYnCb/HIrll8/OzsbqNRFo59MR2to6AIBHjx4CAL77bi2CBgzG0iXh0NXVw8hRQ/D02dOP7dYn9d6xWMw5ee9YFMsfQ+/z7bfhaNmqGXzatUZKcjKWfh1e6jo+tTfn4N2xZ4j0Ys5b4dgrWr648/w+e/fthlcbT3i18cS5c2fxbcQaqKpW7mCfCjFwFbhq1apBTU0NWlpaMDU1hampKZSVlbFy5Uq4ublh4cKFcHBwgJubGzZu3IjTp0/j9u3b0v1dXV0xY8YM2NraIjQ0FBoaGjAyMkJwcDBsbW0xa9YsiMVixMV9+BOtra0tzM3NpbOrMTExaN26NUxNTWFpaYlz585J17+e8SxpO1+7desWTp48ifXr18Pd3R0NGzbEhg0bkJWVVaRsYGAg/P39YWNjg4ULFyIzMxOxsbFQVlaWvvnVqFEDpqamqFZN/kxYdnY2nj17JrMUIO+D54LK15Gjh9HKy0O6vJ7VV4RmzTwxZvR4hC3+Cs1bNsUXPbpJb0wSKRV9S7148QLmzZ+N6dNmoW4dm0/dXIXKy8vFjJlTIJEAkyeFStcXFBQAAAYMGARv77ZwcKiHGdPmQCQCfv75hKKaWyJHjh5Gq9Ye0kWRY/G1gIAB2LZ1F1Z+uwZKykqYM3cGJBKJoptVqXXs0AlbonYgcvUGWFpaYtqMKe/NSa7UlMpxEQA+VeA/6urVqzh9+jR0dHSKbLt79y7s7OwAAC4uLtL1ysrKMDQ0hLPzm3wqExMTAJA7oynP6zzX0NBQxMTEYNKkSQCA1q1bIyYmBs2aNcP58+cRHBxcqna+Fh8fDxUVFTRs2FC6zsbGBvr6+kX2f7tv2tra0NPTK3E/XgsLC8PcuXNl1lmhFazRulT10Mdp1dILTm/dbZ2TmwMAEKeLYWT05k51cXo67GztiuwPANWr60NZWRnp78wkpqeLYWhgBAAwNDRCbm4unj9/JjPrmp6eLvNUgb59AtDHvx/S0lKhq6uHpKRHWLV6BczNzWXqvnT5IkImjsH4cRPxWWffMva+4lSvXv3fcyJ7M0x6enqRGcTXDA2N5J9DQ9nyeXm5mD5zKpJTkrByxVrpbCsAGBkWnu/a1nWk69TU1GBmVgspKckf1aeKVmQs5rxnLNqVYSz+e25Ko3p1fVSvrg8rKytYW9dBF98OuHYtDi4urqWu61N5cw7eHXtiGBi+b+wVLf/u2CsJHR1d6OjowtLCCk5OLvBp3woxZ35Gh/adSl2XognlEn95EUh8TaWVmZkJX19fXLlyRWb566+/0KpVK2m5dy+NiEQimXWvXxCvZ0g+5HWeq1gsxh9//IHWrQsDvNatW+P06dP47bffkJOTI70xq6TtLAt5fStpP14LDQ3F06dPZRZLNP+odlHpaWtrw8LCUrrUqV0XhoZGuHAhVlomMzMTN25cg4uz/D/WqqqqcHBwlNmnoKAAFy7Ewtm58EOOo4MjVFRUZMok3EtAcnISnJ1k6xWJRDA2rgENDQ0cO34UJiamcLB3lG6/dOkCxoeMxqiRY/F59y/L5TyUN1VVVdjbO+LiRdlzcvFSLJycXOTu41TfGRcvxcqsi71wHk7135R/HbQ+uJ+IFeGRqFatukx5BwdHqKmp4V7iPZl9kpIewdS0Zjn0rOIUGYt1ynEsXnwzFstKIil8j8v998NdZaWqqgoHe0dcuHheuk56DooZe85OLjJjFQBiY38vtnxJSSQSSCSFebdU+XHG9T9ATU2tyGM8GjZsiH379sHa2lr6bMlPwdvbGy9evMDy5ctha2uLGjUKnyXYqlUrDBo0CEeOHJGmFJSlnfb29sjLy8Mff/yBRo0aAQDu3LmDJ0+elKqdampqAPDBx5+oq6tDXV32RhylSviy0dRWg7nNm9wv09r6sHE1xbP0LDy+L6ycwZIQiUTw790XGzeth4WFJczNzBG5dhWMjIzRurW3tNzwkUPg7dUGPXv0BgD08Q/A3Hkz4ehYD/XrOWHHzu3IepUF3y7dABTOwnTr2h3fRCyDnl41aGtr4+tli+Ds7CITUGzduhkeHs0hUhLh9OmfEbVlI8IWLoGysjKAwvSA8RNGo3evPmjTxgdp/+YtqqqoFpuWoij+vfpi/lez4eBQD/Xr1cfO3dF49SoLXT4rzMedO38mjI1qYMTw0QCAnj37YMTIYETv2ApPzxY4efIYbt36E1OnzABQGIBOmz4Z8bdvYemSCBQU5EvzNvX0qkFVVRXa2jrw6/YFNnwXCZMaJjA1rYnt0VsAAG282yngLJSddCxufGssRsoZiyP+HYs9/x2LfQIwd+6/Y7H+v2Mx681YBApzpsXpabh//z6Awvc6LW0tmJrURLVq1XD9+jX8+ecNuDZoAD1dPTx48ACRa1ehVi0LOBcTNFcm/v79MG/+LDg61EO9+k7YufPfsffvOZgzdwaMjWtg5IjCR1v16umPYSOCsT16C5p7tsSJk8dw89afCJ06U1rn06dPkZKSjNS0wqtr9xITAACGhoYwNDTCw4cPcOLkMbi7e0C/uj4eP07Blq2boK6uDk+PFp/2BJSTKjbhWgn/AlOpWVtb4/z580hISICOjg4MDAwwcuRIrF+/Hv7+/pg8eTIMDAxw584d7Ny5Exs2bJD+gS1vderUgaWlJb799lv07dtXut7CwgJmZmZYt24d/P39petL204HBwf4+PhgyJAhWLOmMJl+woQJ0NTULNXlEisrK4hEIvz444/o3LkzNDU15aYrCIV9YzOExwyU/jzqm8LLXUc3/4FFQQcU1awK1T8gEFlZWVgYNh+Zmc/h6uqGFRGrZT5oPHx4HxkZbz7UtG/XARkZT7B23RqIxWmws7PHivDVMpcax4+bCJFIhCmhE5CTk4NmzQq/gOBtv507i42bNyA3Nxe2NnZY+nU4mnu++aP340+H8OrVK2yO2ojNURul6xs2bIS1a76riNNRZj4+HfAk4wk2bFhT+BB4W3t8s2yl9MahlJRkKIneXJxzcXbF3DlfYd261YhcuxIWtSyxOGy5NH83NTUV//v1DACgf2BvmWOt+nYdGjZsDAAYPWoclFVUMHf+TGRnZ6N+PSesXLEWenqyN8YJQf/+gch6lYWFC0s5Fp+8MxYjZMfi/v17sH7DWunPQ4YWvsZnzZoL3y7doKGhgdOnT2HdujXIepUFI0MjeHg0x8CBg6Ufziuzdj6F52DdhjUQi8Wws7VH+DerpGkqKSnJUHorb9zFpQHmz12IyHWrsCZyJSwsLLFk8XLUrfsmd/x/v57B/AWzpT/PmDkVADB40FAEDx4GNTU1XLn6B3buisbz589gYGAItwYNsWHd5iI3fglGFYtcRRJmcAtSYGAgMjIycPDgQdy+fRsDBgzA1atXkZWVhX/++QfW1tb466+/MGXKFJw+fRrZ2dmwsrJCx44dsXz5cohEIrnfSGVtbY1x48bJPH6qtN9CFRgYiKioKOzcuRO9evWSrg8KCsLmzZuxY8cO9O795g9aaduZlJSEQYMG4eeff4apqSnCwsIwbtw4zJs3D0OHDi22zdWrV0d4eLj00Vfz58/H6tWrkZKSgv79+2Pz5s0l6p+XaFaJylFRh56EfrgQFSsvr3SpLvSGiioz48qqIJ9hwseobqBVofV/YVN+3za3707IhwspGANXErwHDx7AwsICJ0+eRNu2bSv8eAxcy46B68dh4Fp2DFzLjoHrx2HgWr6YKkCC8/PPPyMzMxPOzs5ISkrC5MmTYW1t/dE3cxEREQmNSKlqpQrwIyiV2LBhw6CjoyN3GTZs2CdrR25uLqZNm4b69euje/fuMDY2RkxMDB8eTUREVY9IVH6LADBVgErs8ePHePbsmdxtenp60icI/NcxVaDsmCrwcZgqUHZMFSg7pgp8nIpOFfjSPrzc6tobP67c6qooTBWgEqtRo0aVCU6JiIiEQCATpeWGgSsRERGRQPGbs4iIiIiIKiHOuBIREREJVRWbgmTgSkRERCRQTBUgIiIiIqqEOONKREREJFBVbcaVgSsRERGRQImq2LVzBq5EREREQlXFZlyrWJxORERERELFGVciIiIigapiE64MXImIiIiESqRUtSJXpgoQERERkSBwxpWIiIhIqKpYrgADVyIiIiKBqmJxK1MFiIiIiKhsVq1aBWtra2hoaMDd3R2xsbHvLR8eHg57e3toamrCwsIC48ePx6tXr0p8PM64EhEREQmUIm/O2rVrF0JCQhAZGQl3d3eEh4ejQ4cOiI+PR40aNYqUj46OxtSpU7Fx40Z4enri9u3bCAwMhEgkwvLly0t0TM64EhEREQmVSFR+SyktX74cwcHBCAoKQr169RAZGQktLS1s3LhRbvnffvsNzZs3R58+fWBtbY327dvD39//g7O0b2PgSkRERETIzs7Gs2fPZJbs7Gy5ZXNycnDp0iX4+PhI1ykpKcHHxwfnzp2Tu4+npycuXbokDVT//vtv/PTTT+jcuXOJ28jAlYiIiEigynPCNSwsDNWqVZNZwsLC5B43LS0N+fn5MDExkVlvYmKC5ORkufv06dMH8+bNQ4sWLaCqqoq6devCy8sL06ZNK3F/GbgSERERCZRISVRuS2hoKJ4+fSqzhIaGlltbY2JisHDhQqxevRqXL1/G/v37cfjwYcyfP7/EdfDmLCIiIiKhKsd7s9TV1aGurl6iskZGRlBWVkZKSorM+pSUFJiamsrdZ+bMmQgICMDgwYMBAM7Oznjx4gWGDBmC6dOnQ0npw/OpnHElIiIiolJRU1NDo0aNcOrUKem6goICnDp1Ch4eHnL3efnyZZHgVFlZGQAgkUhKdFzOuBIREREJlEiB30AQEhKCAQMGoHHjxmjatCnCw8Px4sULBAUFAQD69+8Pc3NzaZ6sr68vli9fDjc3N7i7u+POnTuYOXMmfH19pQHshzBwJSIiIhIoRT7HtVevXkhNTcWsWbOQnJyMBg0a4OjRo9IbthITE2VmWGfMmAGRSIQZM2bg4cOHMDY2hq+vL7766qsSH1MkKencLBEBALxEsxTdBME69KT8kvyrory8AkU3QbBUVJkZV1YF+QwTPkZ1A60KrT+o9fpyq2vTmeByq6uicMaViIiISKAUmCmgEAxciUqJs4Zl11Vf/vMAqWQ49spOTY1/7spKVbVkuYekIFUscuW1EyIiIiISBH4EJSIiIhIoRd6cpQgMXImIiIgEqoplCjBVgIiIiIiEgTOuREREREJVxaZcGbgSERERCZQivzlLERi4EhEREQmUqIolfVax7hIRERGRUHHGlYiIiEiomCpAREREREJQxeJWpgoQERERkTBwxpWIiIhIoPjNWUREREQkDFUsV4CpAkREREQkCJxxJSIiIhKoKjbhysCViIiISKiqWo4rUwWIiIiISBA440pEREQkVFUsV4CBKxEREZFAVbG4lYErERERkVAxx5WIiIiIqBLijCsRERGRQImqWK4AA1ciIiIioapacStTBYiIiIhIGDjjSkRERCRQVe3mLAauRERERAJV1XJcmSpARERERILAGVciIiIioWKqABEREREJQRXLFGCqABEREREJAwNXKjcikQgHDx4sddmEhASIRCJcuXKl2PIxMTEQiUTIyMgAAGzevBnVq1eXbp8zZw4aNGhQpnYTEREJlUgkKrdFCBi4VjKBgYHw8/MDAHh5eWHcuHEKbc+lS5cgEonw+++/y93etm1bfP755wCApKQkdOrUqUT1lqYsAHh6eiIpKQnVqlWTu33ixIk4deqU9Oe3z2NlJpFIELl2NTp29kGLVu4YMWooEhPvfXC/3Xt2oqtfJzRv2RSBA/vhxo1rMtuzs7OxeMlC+LRrjVZeHpg8ZQLEYrFMmdgL5zFwcH+09vZEh05t8e3KcOTl5Um3X7p0ARMmjkPHzj5o2boZ+vTriSNHD5dPxysRl5ZWWHioL/Y+nIgYyTy06Oag6CZVOEWOu6XLFiOgvz88WzRBn349ixwj4V4Chg0fjA4d26B5y6bo1v0zrIlciby83I/rdAWSSCRYtXol2vp4oal7IwwZOhj37n34fO7cuQOdOrVHk6YN0befP65de3M+Hz58CNcGTnKX48ePScvJ237k6E8V0s9PRSKR4NtvV6BV65Zwa9gAAwcFIeFewnv3uXjxAkaMGI7WXq1Qr74jTp46WaTMylUr8VmXzmjUuCGaebhj4KAgXI27WkG9+ISUROW3CAADV3qvRo0awdXVFRs3biyyLSEhAadPn8agQYMAAKamplBXVy9RvaUpCwBqamowNTUt9hOhjo4ODA0NS1xfZbFl62bs2h2N0CnTsem7rdDU0MTosSOQnZ1d7D7HTxxDeMQyDB40FFujdsDWxg6jx45Aenq6tMw34Uvxv19/QVjY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j3SdkpISfHx8cO7cObn7HDp0CB4eHhg5ciRMTEzg5OSEhQsXIj8/v8RtZOBKREREJFAiJVG5LWFhYahWrZrMEhYWJve4aWlpyM/Ph4mJicx6ExMTJCcny93n77//xt69e5Gfn4+ffvoJM2fOxLJly7BgwYIS95epAkRERERCVY6ZAqGhoQgJCZFZp66uXm71FxQUoEaNGli3bh2UlZXRqFEjPHz4EF9//TVmz55dojoYuBIRERER1NXVSxyoGhkZQVlZGSkpKTLrU1JSYGpqKnefmjVrQlVVFcrKytJ1jo6OSE5ORk5ODtTU1D54XKYKEBEREQmUSCQqt6U01NTU0KhRI5w6dUq6rqCgAKdOnYKHh4fcfZo3b447d+6goKBAuu727duoWbNmiYJWgIErERERkWCVZ45raYWEhGD9+vWIiorCzZs3MXz4cLx48UL6lIH+/fsjNDRUWn748OFIT0/H2LFjcfv2bRw+fBgLFy7EyJEjS3xMpgoQERERUan16tULqampmDVrFpKTk9GgQQMcPXpUesNWYmIilJTezJFaWFjg2LFjGD9+PFxcXGBubo6xY8diypQpJT6mSCKRSMq9J0T/Yc8yshTdBMHqqi//7lQqmUNPQj9ciORSU+c8TVmpqip/uBAVS1mlYi9uz515otzqmj2/XbnVVVH4SiYiIiISKsV9/4BCMMeViIiIiASBM65EREREAqXIr3xVBAauRERERAJVxeJWBq5EREREQlXVAlfmuBIRERGRIHDGlYiIiEigmONKRERERIJQxeJWpgoQERERkTBwxpWolPLyChTdBMHiNz99HH7zWNl9nz5V0U0QrK8Xn1F0EwRt5hyfCq2fqQJEREREJAhVLG5lqgARERERCQNnXImIiIgEiqkCRERERCQIVSxuZaoAEREREQkDZ1yJiIiIBEqEqjXlysCViIiISKCqWqoAA1ciIiIigapqgStzXImIiIhIEDjjSkRERCRQfBwWEREREQlCFYtbmSpARERERMLAGVciIiIioapiU64MXImIiIgEqorFrUwVICIiIiJh4IwrERERkUDxqQJEREREJAhVLG5lqgARERERCQNnXImIiIgEiqkCRERERCQIVSxuZeBKREREJFRVLG5ljisRERERCQNnXImIiIgEijmuRERERCQIVSxuZaoAEREREQkDZ1yJiIiIBIqp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xx9/RM+ePaFSsauPqBH/GoiIiEyQt7c3zp8/LzoGkUlh4UpERGSC+IUoUVMsXImIiIhIEVi4EhEREZEisHAlIiIiIkVg4UpERGSCOJyAqCkWrkRERCaIm7OImmLhSkREJKOIiAjcuHGjyfWqqipEREQYXp88eRLu7u5yRiMyeRxAQEREJCMLCwuUlpbC0dHR6PrPP/8MJycn1NfXC0pGZPo6iA5ARETUHly/fh16vR56vR43btyAlZWV4Z5Op8PevXubFLNEZIyFKxERkQy6du0KSZIgSRL69evX5L4kSVi2bJmAZETKwVYBIiIiGXz99dfQ6/UIDAzEzp070a1bN8M9tVoNd3d39OzZU2BCItPHwpWIiEhGJSUlcHNz43FXRH8CTxUgIiKSkbu7O7KysjBjxgyMHj0aFy9eBAB88MEHyMrKEpyOyLSxcCUiIpLRzp07MWHCBFhbW+Po0aO4desWAODatWtISEgQnI7ItLFwJSIiktHKlSuxceNGvP3227C0tDRcHzNmDI4ePSowGZHpY+FKREQko9OnT8Pf37/J9S5duqCyslL+QEQKwsKViIhIRk5OTigsLGxyPSsrC56engISESkHC1ciIiIZRUVFYd68ecjJyYEkSfjpp5/w0UcfIT4+HrNnzxYdj8ikcQABERGRjBYtWoSGhgYEBQWhuroa/v7+6NixI+Lj4zF37lzR8YhMGs9xJSIiEqC2thaFhYXQarXw9vZG586dRUciMnksXImIiIhIEdgqQERE1MaCg4Nb/WxycnIbJiFSNhauREREbaxLly6iIxCZBbYKEBEREZEi8DgsIiIiGQUGBjY7aOD69esIDAyUPxCRgnDFlYiISEYqlQplZWVwdHQ0un758mW4uLigrq5OUDIi08ceVyIiIhnk5eUZfj558iTKysoMr3U6HVJSUuDi4iIiGpFicMWViIhIBiqVCpIkAQCae+u1trbG+vXrERERIXc0IsVg4UpERCSDkpIS6PV6eHp64ttvv4WDg4PhnlqthqOjIywsLAQmJDJ9LFyJiIiISBHY40pERCSjpKSkFu+HhYXJlIRIebjiSkREJCN7e3uj13V1daiuroZarYaNjQ0qKioEJSMyfTzHlYiISEZXr141+qfVanH69Gncc8892Lp1q+h4RCaNK65EREQm4MiRI5gxYwZOnTolOgqRyeKKKxERkQno0KEDfvrpJ9ExiEwaN2cRERHJaNeuXUav9Xo9SktLsWHDBowZM0ZQKiJlYKsAERGRjFQq4y87JUmCg4MDAgMD8eqrr8LZ2VlQMiLTx8KViIhIgCtXrgCA0SACImoZe1yJiIhkUllZiejoaHTv3h1OTk5wcnJC9+7d8dxzz6GyslJ0PCKTxxVXIiIiGVRUVGDUqFG4ePEipk+fjoEDBwIATp48iY8//hiurq7Izs5ucs4rEf2ChSsREZEM5s+fj/379+Orr75Cjx49jO6VlZVh/PjxCAoKwpo1awQlJDJ9LFyJiIhk4OHhgbfeegsTJkxo9n5KSgqeffZZnD9/Xt5gRArCHlciIiIZlJaWYtCgQXe87+Pjg7KyMhkTESkPC1ciIiIZdO/evcXV1OLiYnTr1k2+QEQKxMKViIhIBhMmTMCLL76I2traJvdu3bqFJUuW4IEHHhCQjEg52ONKREQkgx9//BHDhw9Hx44dER0djQEDBkCv1+OHH37AG2+8gVu3buHIkSNwdXUVHZXIZLFwJSIikklxcTHmzJmDtLQ0NL79SpKE+++/Hxs2bEDfvn0FJyQybSxciYiIZHb16lWcPXsWANC3b1/2thK1EgtXIiIiIlIEbs4iIiIiIkVg4UpEREREisDClYiIiIgUgYUrERERESkCC1ciIiIiUgQWrkRERESkCCxciYiIiEgR/h/iUR1D0i8YlAAAAABJRU5ErkJggg==\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Explanatory Visuals (Part 4)"
      ],
      "metadata": {
        "id": "sXG8kHd0-lJT"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df.info()"
      ],
      "metadata": {
        "id": "DEGOvrxX-pe_",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "1258fa3e-3c41-4bc1-a9bf-754f5bdc1344"
      },
      "execution_count": 35,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 8523 entries, 0 to 8522\n",
            "Data columns (total 12 columns):\n",
            " #   Column                     Non-Null Count  Dtype  \n",
            "---  ------                     --------------  -----  \n",
            " 0   Item_Identifier            8523 non-null   object \n",
            " 1   Item_Weight                8523 non-null   float64\n",
            " 2   Item_Fat_Content           8523 non-null   object \n",
            " 3   Item_Visibility            8523 non-null   float64\n",
            " 4   Item_Type                  8523 non-null   object \n",
            " 5   Item_MRP                   8523 non-null   float64\n",
            " 6   Outlet_Identifier          8523 non-null   object \n",
            " 7   Outlet_Establishment_Year  8523 non-null   int64  \n",
            " 8   Outlet_Size                8523 non-null   object \n",
            " 9   Outlet_Location_Type       8523 non-null   object \n",
            " 10  Outlet_Type                8523 non-null   object \n",
            " 11  Item_Outlet_Sales          8523 non-null   float64\n",
            "dtypes: float64(4), int64(1), object(7)\n",
            "memory usage: 799.2+ KB\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "1. Which Product Type earns the most and least money?"
      ],
      "metadata": {
        "id": "VooEz9Sl-EG3"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "means = df.groupby('Item_Type')['Item_Outlet_Sales'].mean().sort_values(ascending=False)\n",
        "means"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "NLkfsOgH7Nnt",
        "outputId": "4ac9b1b3-bf4a-4abc-e9f8-f772cfb4c159"
      },
      "execution_count": 36,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Item_Type\n",
              "Starchy Foods            2374.332773\n",
              "Seafood                  2326.065928\n",
              "Fruits and Vegetables    2289.009592\n",
              "Snack Foods              2277.321739\n",
              "Household                2258.784300\n",
              "Dairy                    2232.542597\n",
              "Canned                   2225.194904\n",
              "Breads                   2204.132226\n",
              "Meat                     2158.977911\n",
              "Hard Drinks              2139.221622\n",
              "Frozen Foods             2132.867744\n",
              "Breakfast                2111.808651\n",
              "Health and Hygiene       2010.000265\n",
              "Soft Drinks              2006.511735\n",
              "Baking Goods             1952.971207\n",
              "Others                   1926.139702\n",
              "Name: Item_Outlet_Sales, dtype: float64"
            ]
          },
          "metadata": {},
          "execution_count": 36
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "from matplotlib.ticker import FuncFormatter\n",
        "fig, ax = plt.subplots(figsize=(20,10))\n",
        "\n",
        "ax = sns.barplot(data=df,x='Item_Type', y = 'Item_Outlet_Sales', order = means.index, ci = None)\n",
        "plt.xticks(rotation = 90)\n",
        "ax.set_title('Average cost vs. Product type', fontsize = 20, fontweight = 'bold');\n",
        "ax.set_xlabel('Item Type', fontsize = 15, fontweight = 'bold')\n",
        "ax.set_ylabel('Item Outlet Sales', fontsize = 15, fontweight = 'bold');\n",
        "\n",
        "def hundred_k(x,pos):\n",
        "  \"\"\"function for use with matplotlib FuncFormatter - formats money in millions\"\"\"\n",
        "  return f'${x*1e-3:,.0f}K'\n",
        "\n",
        "price_fmt_100k = FuncFormatter(hundred_k)\n",
        "\n",
        "ax.yaxis.set_major_formatter(price_fmt_100k)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 786
        },
        "id": "Qwqg0RM68Sy5",
        "outputId": "8d0f1fd5-9a15-4ae7-f2ae-96c29c24760c"
      },
      "execution_count": 37,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "<ipython-input-37-9560d6bfc182>:4: FutureWarning: \n",
            "\n",
            "The `ci` parameter is deprecated. Use `errorbar=None` for the same effect.\n",
            "\n",
            "  ax = sns.barplot(data=df,x='Item_Type', y = 'Item_Outlet_Sales', order = means.index, ci = None)\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 2000x1000 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAABmAAAAPtCAYAAACZ66DgAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/bCgiHAAAACXBIWXMAAA9hAAAPYQGoP6dpAADWs0lEQVR4nOzdd5iU1d0//s8sSJGqVEGKBSyIYuyVomJB0SjGoFI0T0SjqKDGR58YY2KJSR4fYozRJIrB3hvYiFIs2AJGQRFUmm6wIEVB6t6/P/yxX4aZhd3hht2F1+u65gpz7nOf+zOzMztm3nvOySRJkgQAAAAAAACpKarsAgAAAAAAADY3AhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAKqVTCaTdWvfvn1llwQAADkEMAAAVdjQoUNzvmjMZDJRt27dWLRoUWWXB1SSsWPH5v3dsOatZs2ase2220bnzp3j7LPPjueeey5KSkoqu3QAANhiCGAAAKqokpKSeOCBB/IeW7p0aTz22GObuCLYOLp165YTHsycObOyy6r2Vq1aFfPnz4/JkyfH8OHD49hjj41DDjkkPv7448oubYu1MV/r7du3zxkbAIDKJYABAKiiXnrppfjPf/5T5vF77rlnE1YDbA5ef/316NatW3z22WeVXQoAAGz2alZ2AQAA5Hfvvfeu8/iYMWOiuLg4WrVqtYkqAqq6iy66qPTfxcXFMW7cuPjiiy+y+nz66afx85//fL2/YwAAgA0jgAEAqIK+++679S4xVlJSEvfff39ccsklm6gqoKobNmxY1v0FCxZEr1694rXXXstqf/jhh+Mvf/lLNGzYcBNWBwAAWxZLkAEAVEFPP/10LFq0KKutT58+UVSU/Z9v+f6CvaSkJNq2bZu1D0CzZs1i5cqVZV6vQ4cOWf3r168f3377bd6xH3nkkejXr1/ssssu0bhx46hdu3a0atUqjjnmmPjzn/8c3333XZnXmTlzZs4eBd26dYuIiKlTp8agQYNixx13jDp16mTtjbB8+fJ4/vnn47rrrosf/vCHsddee0WbNm2iXr16UatWrWjatGnsu+++ce6558a4cePKvP6aiouL46KLLoqddtop6tSpEy1atIjjjjsunnrqqYiIuOuuu3Jq/dWvfrXOMd94440YPHhw7L333tGsWbOoVatWNGvWLA488MD45S9/GcXFxeWqrbyWLVsWI0aMiL59+0aHDh1Kfx7bb799HHDAAXHJJZfE888/v84xXn755TjvvPNizz33jCZNmsRWW20VTZo0ib322ivOP//8mDBhwnrr+OSTT+LKK6+MQw45JFq0aBG1a9eOunXrRps2bWLvvfeOH/3oR3HDDTfEq6++WroJ/JqvhXw/sx122KHgvTIWLVoUW2+9dda5e++9d5n9v/vuu2jYsGFW/x133DGSJCnts2rVqnjggQfi1FNPjV122SUaNGgQNWvWjCZNmsQuu+wS3bp1i8GDB8e9994bc+fOLVedm0Ljxo3j97//fU77ihUr4q233iq9X+h7c01z586Na6+9Nnr06BGtWrWKOnXqRP369aN9+/bRp0+fuPPOO2PZsmXrrXnVqlVx6623xoEHHhiNGjWKhg0bxj777BN/+MMfynX+agMHDsx5TGPHjs3pN3bs2Jx+AwcOXOfYs2fPjmuuuab0sdatWzcaNGgQHTp0iGOOOSb+93//Nz766KOI2Liv9TVrnzVrVs7xtcfNZDKRJEnssssuWW3bbLNNLF26tMzr7L777uv8jChr/5nFixfH9ddfH3vvvXc0bNgwGjZsGPvtt1/88Y9/jBUrVqz38W3oZw4AQKVLAACocnr37p1ERNZtzJgxyeGHH57T/v777+ecf+WVV+b0e+655/Je6+23387pO2DAgJx+kyZNSnbdddecvmvfWrVqlYwbNy7vtWbMmJHTv2vXrsn999+f1KlTJ+fYjBkzkiRJkvfee2+9113zdvzxxycLFiwo8/kdN25c0qhRozLPP+uss5I77rgjp/3qq6/OO96XX36ZHH/88eutq27dusmf/vSnMuuqiFGjRiXbbbdduZ6PfObOnZscddRR5X4+v/rqq7zj3HHHHclWW21V7p/NnDlzkiTJ/1pY323166E8Tj/99Jzzp06dmrfvI488ss6f9VdffZXsv//+5a7zJz/5SbnrLNSYMWPK/bP+5ptv8va9//77S/sU+t5MkiQpKSlJbrjhhqR27drl+v3w4osvlvm4Fi5cmBxyyCFlnt+pU6dkzpw5Oe3t2rXLGWvAgAE5/caMGVOu5zLf78AkSZJly5YlF198cVKzZs31PtbVY2zM13pZr4P1/T4YNmxYTvs999yT9xqTJ0/O6du/f/+sPu3atcvpM23atGTHHXcss4599tknmTdvXpmPLY3PHACAymYGDABAFfP111/Hs88+m9XWokWLOPzww+OUU07J6X/PPffktA0YMCCn7cEHH8x7vXzta//194QJE+Lggw+OqVOnrqv0iPh+ZsmRRx4ZL7744nr7Rnz/1/X9+/df519fV9TIkSPjjDPOyHvso48+il69esXChQvLPH/48OFx7bXXluta8+bNi4MOOihGjhy53r7fffddDB48OH7zm9+Ua+yy/P3vf4/jjz8+/vOf/xR0/ueffx4HHHBAjB49ulz9R44cGQcffHAsWLAgq33atGlx7rnnlusv2Te1fDMYyvseyGQyWe+hoUOHxptvvplqfZvS/Pnz87bXqVNnneeV9735s5/9LK644opyzU4pLi6Onj175n2/JEkSffr0iVdffbXM86dMmRLHHHPMeq+zMaxYsSKOO+64GDZs2DpnFFYHAwcOjHr16mW13XHHHXn7PvLIIzlt+T5j1nbkkUfGJ598Uubxf/3rX9G7d+/SWXFr2pifOQAAm5I9YAAAqpiHHnoo5wvtH/7wh1FUVBSnnHJKXHzxxVlLI913331x7bXXli75EhHRsWPHOOigg7KWj3r88cfjtttui1q1auVcb03t27ePrl27lt7/9ttv4+STT85Z5mW77baLrl27Rr169eKtt96Kd999t/TYihUrom/fvjFt2rRo3LjxOh/v559/HhERNWvWjB49esSOO+4YX375ZYwZMyanb506daJLly7RrFmzaNq0adSvXz+++eabeP/99+Ott97Kel5GjRoV48ePj8MPPzxrjPPPPz/v8mpHHHFEdOjQId5777149dVXY8aMGeuse7WBAweWLjW0Wt26daNnz57RsmXL+Oijj+Kll17Kqu3qq6+Obt26xWGHHVaua6xp0qRJcd5552WNt9rBBx8ce+yxR5SUlMQHH3wQb7zxRt4vivv375+zXFFRUVH07Nkzdthhh5g+fXq8+OKLWdeYNm1aDBo0KCusyPda7dChQxx88MFRv379WLRoUUyfPj3ee++9WLx4cVa/hg0blm4Y/8gjj8Rnn32Wdfyss87K2Z+kIvuVHHHEEbH99tvHp59+Wtr24IMPxi9/+cusfosXL45Ro0ZltR1++OGxww47RMT3r+WHH34463jt2rXjiCOOiLZt28aqVati7ty5MXny5HK/Zja1svaT2nHHHdd5Xnnem/fff3/cdtttOefusssu0bVr11i8eHE888wzWSHQqlWr4vTTT4/p06dHixYtStvvueeevKHgTjvtFEcccUQsWrQoRo0aFVOmTFn3A95I/vu//zvvl/wNGzYsXYpswYIF8frrr2cFDxvztb799tuXjn3nnXfGN998k3V89bG1NWrUKM4444z461//Wto2duzY+OSTT3JeF2sHMG3atInu3buvt7bZs2dH/fr147jjjovGjRvH+PHjcwKVV199NW6//fY477zzSts29mcOAMAmVZnTbwAAyHXooYfmLLHyz3/+s/T4QQcdlHP85Zdfzhnn9ttvz+n31FNPZfWZMGFCTp9f/vKXWX1uvPHGnD5nnHFGsnTp0qx+v/zlL3P6XXPNNVl9ylqKp0mTJsm//vWvrL7Lli1Lli9fniRJksybNy95/vnnkyVLlpT5vD322GM541588cVZfaZMmZL3+sOHD8/q97vf/S5vv7WXIHv99ddz+nTq1CmZO3duVr/nn38+Z8mi7t27l/lY1qVXr14512zRokUyYcKEnL6ffvpp8rOf/Syr7dVXX805v2bNmskLL7yQ1e+xxx5LMplMVr9MJpO89957pX1++tOfZh0/4IADkpUrV+bUsXz58mT8+PHJ4MGDky+++CLneNeuXQtegmldrrjiipxx33333aw+999/f06fO++8s/T4Z599lnN85MiRea/36aefJrfffnty6623bnDt61OeJciKi4uTW265JalXr15Ov+233z4pKSkp7Vvoe7Njx44555x99tlZr4PPP/88b7/LLrssa9x99tknp8+JJ55Yeq3VdeZbem9jL0H22WefJbVq1crp17t372T+/Pk5Y7700kvJsGHDcto31ms9SfIvA7Yu//73v3P6/8///E9Wnw8//DCnz5VXXlmua7do0SL5+OOPS/usXLkyOfPMM3P6dezYMWusND9zAAAqmwAGAKAKmTlzZs6X3k2bNs36MvN///d/c750Ovfcc3PGWrBgQc7eDaeffnpWn4svvjjnC/Y1vzBLkiTZb7/9svrUrl077xeOK1asSOrWrZvVt3Pnzll9yvqS9x//+Ee5n6MpU6Yk99xzT/Kb3/wmufzyy5OLL744ueiii5ILL7wwZ9zDDz8869zf//73OX322WefnGusWrUq794Fawcwl112WU6fZ555Jm/da++3kslkytxXpSyLFi3Ku99KWdfM55JLLsk5/6yzzsrb96STTsrpe91115UeHzp0aNaxTp06JV9//XWFHlOSbLwvpadOnbreL4/Xfoz16tVLvvnmm9LjixYtyhnj9ttv3+DaNlQhe3+sefvrX/+aNV4h7818ezM1aNAg6/lb7Yknnsjp26FDh9LjX3zxRc7xoqKi0j2D1vTnP/85p+/GDmD+8pe/5PRp3779OkPhfKpSAJMkSXLYYYdl9W/dunXW5821116bM+aHH35Yrmvn2+/q66+/zrtX0EcffVTaJ83PHACAymYPGACAKuS+++7LWVrqpJNOiho1apTez7cPzMMPP5yzFFSjRo3ipJNOymp76qmnSpd1SZIkZ2mlww47LGv5mVWrVsW//vWvrD7Lli2LbbbZJjKZTNZtq622ylkyZvLkyXmX+1pTnTp14rTTTltnn4jvl8HZddddo1OnTnHmmWfGVVddFTfeeGMMGzYs/vjHP8bNN9+cc85XX32Vdf+dd97J6fPDH/4wp62oqChv+9reeOONnLbjjjsu57nJZDI5SyslSRKvv/76eq+xpokTJ+b8nNu2bRvHHntsucd46623ctp69eqVt2++9jXPP+KII7KOTZkyJVq2bBk/+MEPom/fvvGrX/0qHnrooZwllzaVXXbZJQ488MCstjWXUFu0aFHOfkunnHJK1K9fv/R+gwYNYr/99svqM2jQoGjdunX07NkzLrjggrjlllvitddei+XLl2+ER5G+8847L37605+ut9/63pv5Xktdu3bNev5WO/roo7N+j0VETJ8+vXRfoXzvzU6dOsX222+f014Ze8Dke6/3798/6tatu8lrSdP555+fdf+zzz6L559/vvT+2suPHXDAAdGxY8dyjZ3v99I222wT+++/f0776p//pvjMAQDYlAQwAABVyL333pvTduqpp2bdb9euXey7775ZbfPmzcv5Ijkid6Pkb7/9tnS/i5dffjnni/G1Ny6fN29e3g2SyytJktJ9JMrSsWPHqF279jr7/OlPf4pTTz01Pvzwwwpdf8mSJVn3582bl9OnTZs2ec8tq31NX375ZYXqWdvcuXMr1D/fc7nrrrtWaIx8NVfkOfjiiy9K/33cccfl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Vc5ygBHfB3Frvvcq+zMHACBNZsAAAFQB+ZYfu+qqq7LW5V/bt99+G82aNYulS5eWto0ZMyaKi4ujVatWWX0HDhwYb731Vplj9evXr8zlhi644IL44x//mPUl85tvvhnt27ePrl27Rrt27aJWrVrx9ddfx9SpU+ODDz4o/cJ03333LfOa5bX2l7IREXfffXdMnTo1fvCDH8TcuXNj9OjRsWTJkvWOtdtuu0XPnj3jhRdeKG1LkiROPvnk6NmzZ+y4444xefLkePnll8tV20EHHRTHHntsPPvss6Vt8+bNi4MPPjj22Wef2HXXXaNx48axcOHC+OSTT+Ldd98t3Z+ga9eu5brG2n7961/Hc889l/UF9+effx4HHXRQHHzwwbHHHntEkiQxbdq0eO2112L58uVZXwQffPDBOc/BypUr4+ijjy59DqZPnx7//Oc/c/bQOfXUU2OPPfYovf/ll1/G9ddfH9dff300a9YsOnfuHG3bto369evHN998ExMmTIhp06ZljdG4ceOcpavWnj0S8f1yYMcff3zp0lSdOnWKn/70pwU8Y/9vKb7f//73ZfZZ11/3jx49Ou66666oWbNm7L777tGxY8fSpZJmzZoVL730Us45awcTm6tf/epXcfrpp2e13XHHHfHKK69E165dY8mSJTFq1KiYP39+Vp8GDRrEJZdcktV24YUX5vwcHn/88dh9992jR48e8c0338SoUaPK3INnbfneY6+//nrsv//+cdhhh8WHH36Y92eXT+vWreP888+P//u//8tqf/LJJ6Nt27ZxxBFHRKtWrWLRokXx1ltvxdSpU/O+pjbma71Dhw45YejRRx8dxxxzTOnSlIcddlje2W5du3aNTp065d2HLCKib9++UbNmxb4++Pzzz2OvvfaKXr16RePGjWP8+PE5yx9GZO9bFFH5nzkAAKlKAACoVEuXLk0aNWqURETW7Z133lnvuSeccELOeX/4wx9y+s2bNy+pXbt2Tt/Vt48++mid13n11VeTOnXqlHl+Wberr746a5wZM2bk9Onates6r71q1apkr732Wu+1jj766Jy2du3a5Yw3ffr0pH79+usdb/fdd1/v40mSJPniiy+SnXbaqcLPzfoe97rcdtttFbrW2ubOnZu0a9euQmN07NgxmT9/ftY4f/vb3yr8uC+//PKcel588cX1nterV6+Cn68kSZLJkyeXOfbWW2+dLFq0qMxzW7duXaHH2KhRo+Q///lP1hiFvPbXZcyYMeX6WZfXhtR37rnnVuj5qVGjRvLUU0/ljFNSUpL06NFjvee3adOmXO/1JEmSgw46aL3jNW3aNKdtwIABOWMtW7Ys6dq1a7kfZ74xNuZr/Y477ljv2Oeff36Z5996661lnjdx4sR1Xjvf75N8v0PXvh144IHJypUrc8ZL6zMHAKCyWYIMAKCSjRw5MmcJm5122ilrj4my/PCHP8xpyzebZtttt40TTjgh7xjrWnpptYMPPjgmTJgQnTp1Wm9Nq2233XblegzrU1RUFA899FDOPhBrOuigg8q1j0NExM477xyjRo2KRo0aldnnggsuiCFDhuS0165dO6etWbNm8frrr8eJJ55YrutHRGy99dZx2GGHlbv/2gYNGhRPPvlktGjRoqDzW7RoEa+//nrWXi3r0qtXr3jttdey9m2JWP8m7Ws7/fTT45prrslp79GjR4Wev0J06tSpzL+OP/nkk6NBgwZlnluRx9m0adN4/PHHo2XLlhWusbq69dZb4/rrr8/7/lhbq1at4oUXXsj7+yiTycSjjz4aBx54YJnnt2vXLkaPHl3u2oYPH77O98lRRx0Vd955Z7nGqlWrVjz//PMxePDggvcZ2Ziv9TPPPHOdezStT79+/Upnyqypc+fOsffee1d4vOeeey523333Mo/vvffe8fTTT+d9LivzMwcAIE2WIAMAqGT5ApN8S8Tkc8IJJ0SNGjWylqOaNGlSfPDBB7Hbbrtl9R0wYEA88sgjOWMMHDiwXNfq0qVLvPfeezFq1Kh4/PHH44033oji4uJYtGhR1KlTJ5o2bRodO3aM/fbbL4466qg47LDDUtsMuWPHjjFp0qT43e9+F08++WTMmjUr6tWrFx07dozTTz89zjvvvNhqq63KPd7hhx8e77//fvz2t7+NkSNHRnFxcTRq1Cj23XffOP/88+O4447Lu/zb2ktnrda0adN44okn4p133om77747Xn311ZgxY0YsWLAgioqKonHjxqWh2hFHHBE9e/bMu19DRfTu3Tt69uwZDzzwQDz77LPx9ttvx5dffhlLly6NZs2aRevWrePQQw/Nu2dMRETLli1j9OjRMX78+Ljvvvvi1Vdfjc8++yy++eabaNCgQbRp0yYOPfTQOPPMM+Oggw7KO8bZZ58de+21V7z00kulyy6tHiOTyUSDBg1ihx12iAMOOCBOP/30OOSQQ8p8PI888kjceuut8cADD8T7778fixYtylkCbUMNGDAga3Pv1db3Hvj3v/8do0ePjtdeey3eeeedmDlzZnz55ZexbNmyqFOnTjRv3jx23333OOaYY2LAgAF5v8TenGUymbjiiivirLPOir///e/x4osvxtSpU+Prr7+OmjVrlu7v0qtXrzjjjDPWGdQ0btw4Xn755bj99tvjH//4R+mSVTvssEOcfPLJcckll6wzLFvbLrvsEhMnTozrr78+Ro0aFcXFxdGgQYPYe++94+yzz46+ffvG2LFjyz1e7dq14+abb46hQ4fG8OHDY9y4cfHhhx/G/Pnzo2bNmtGyZcvYeeed48gjjyxzn6eN9VqvVatWjBkzJm666aZ44oknYvr06fHtt9+We+z69evHgAED4k9/+lNWe//+/Quqp02bNvH222/HzTffHA8++GB89NFHkSRJdOzYMc4888y44IIL1vl7uzI/cwAA0pJJ0v5/NQAAsBnYZ599YuLEiVltb7755gb9hTlAVXbLLbfE4MGDS+/XqFEj5syZE9ttt906z2vfvn3MmjUrq81XDQAAEZYgAwBgi3PJJZfEK6+8kvcLwqVLl8bgwYNzwpdWrVrFPvvss6lKBNikFi9eHLfccktW2zHHHLPe8AUAgLJZggwAgC3Oo48+GjfddFO0aNEi9t1339h+++2jRo0a8dlnn8X48eNj/vz5OedcffXVUVTk75eAzcd9990Xb775ZixatCheeumlnFksa86GAQCg4gQwAABssT7//PMYNWrUevv169cvfvrTn26CigA2nRdeeCH+8Y9/5D3WvXv3MveQAgCgfPwJHwAAlKFOnTpxww03xPDhwyOTyVR2OQCbRNu2bcsMZgAAKD8zYAAA2OL885//jMceeyxefvnlmDFjRnzxxRcxf/782HrrraNp06ax5557Rrdu3aJfv36x7bbbVna5ABvdVlttFW3atIkTTjghfvGLX0TTpk0ruyQAgGovk+TbeRQAAAAAAICCmQGzDiUlJVFcXBwNGjSw5AQAAAAAAGzhkiSJb775Jlq1ahVFReve5UUAsw7FxcXRpk2byi4DAAAAAACoQubMmRPbb7/9OvsIYNahQYMGEfH9E9mwYcNKrgYAAAAAAKhMixYt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ZmUXAAAAAAAApOuLP42p7BKqneaDu6c6nhkwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKSsogFm+fHkUFxdHcXFxfPPNN6XtI0eOjOOOOy46deoUJ510UkycODG1QgEAAAAAAKqLggKY22+/Pdq0aRNt2rSJBx98MCIiRo0aFSeeeGI8//zzMXXq1Hjqqaeie/fuMWPGjFQLBgAAAAAAqOoKCmBef/31SJIkIiJ69eoVERF/+tOfSttW/++3334bf/zjH9OoEwAAAAAAoNooKIB55513IiJihx12iO222y5WrVoVL7/8cmQymWjdunUcfvjhpX1feumlVAoFAAAAAACoLgoKYL744ovIZDLRpk2biIj46KOP4rvvvouIiFtuuSXGjh0bHTp0iCRJYubMmakVCwAAAAAAUB0UFMAsXLgwIiIaNWoUERFTp04tPbbvvvtGRMRuu+0WERFLly7doAIBAAAAAACqm4ICmK233joiIj744IOIiHj77bcjIqJu3brRqlWriIhYuXJlREQ0btx4Q2sEAAAAAACoVmoWctLOO+8cEydOjI8++ih23333mDFjRmQymdhzzz1L+8yePTsiIpo3b55OpQAAAAAAANVEQTNgevXqVfrvDz/8MJYtWxYRESeccEJERMyfPz+mTp2aE8oAAAAAAABsCQoKYIYOHRq77bZbJEkSSZJERMSee+4ZF1xwQUREPPHEE6VLkB122GEplQoAAAAAAFA9FLQEWaNGjeLtt9+Oxx9/PD799NPYeeed4/jjj49atWpFRESXLl3i4YcfjoiIrl27plctAAAAAABANVBQABMRUbdu3Tj99NPzHtt7771j7733LrgoAAAAAACA6qzgAGZNM2bMiFmzZsWSJUviuOOOS2NIAAAAAACAamuDAphHHnkkrrrqqpg2bVpERGQymVi5cmVcfPHF8e6770bNmjXjiSeeiK233jqVYgEAAAAAAKqDokJPvPbaa+O0006LadOmRZIkpbeIiE6dOsXYsWPjxRdfjMcffzy1YgEAAAAAAKqDggKY1157La6++uqIiNLQZU2nnHJKFBV9P/Tzzz+/AeUBAAAAAABUPwUFMDfffHNp8LLvvvtGx44ds45vu+22scsuu0SSJDFx4sQNrxIAAAAAAKAaKSiAeeWVVyIiokWLFjF+/Pjo3LlzTp8ddtghIiLmzJmzAeVFDBw4cIPOBwAAAAAA2NQKCmC+/PLLyGQysc8++0SdOnXy9ikpKYmIiO+++67w6vJYsWJFXH755dG5c+eoV69etGrVKvr37x/FxcVZ/TKZTDzxxBNZ5/Xt2zdat24dkydPTrUmAAAAAACANRUUwNSrVy8iIr7++uu8x5Mkiffffz8iIho2bFjh8b/66qsYMGBAtG3bNu6///7Yeeed49RTT43ly5fHkiVLYuLEiXHVVVfFxIkT47HHHosPP/wwevfuXeZ4S5Ysid69e8dbb70Vr7zySuyxxx4VrgkAAAAAAKC8ahZyUseOHePNN9+MN954I15//fWc47/73e9i9uzZkclkYrfddqvw+EOGDIk333wz7r777hg2bFhceOGF8dxzz0VJSUk0atQoRo8endX/lltuif333z9mz54dbdu2zTq2YMGC6NWrV3z77bfxyiuvRMuWLStcDwAAAAAAQEUUFMD06tUr3nzzzUiSJA4//PDSGTERER06dIhPPvkkq29FTZo0Kfr37x9du3aN4cOHR/fu3aN79+5l9l+4cGFkMplo3LhxVvvcuXOja9euUb9+/Rg3blzO8bUtW7Ysli1bVnp/0aJFFa4dAAAAAACgoCXIzj///GjevHlERKxcuTIWLVoUmUwmIiI+/vjjSJIkIiKaN28e5557boXHP+SQQ2L48OExcuTI9fZdunRpXH755dG3b9+c5c4uuuiiWL58eYwePXq94UtExA033BCNGjUqvbVp06bCtQMAAAAAABQUwGy77bbx+OOPx7bbbptzbHUQ07hx43j00UfLFXys7aabborTTjsthgwZEiNGjIguXbrEbbfdltNvxYoV8aMf/SiSJIm//OUvOcePP/74mDZtWtx+++3luu4VV1wRCxcuLL3NmTOnwrUDAAAAAAAUFMBERBx00EExefLkGDp0aOy6665Rp06dqFOnTuyyyy4xZMiQmDJlShx88MEFjV2vXr247rrrYvr06dG7d+8477zzYujQofHXv/61tM/q8GXWrFkxevTonNkvERH9+vWLO++8My699NK46aab1nvd2rVrR8OGDbNuAAAAAAAAFVXQHjCrtWjRIv7whz/EH/7wh7TqydG4ceMYNGhQvPDCC/Hyyy/HOeecUxq+TJ8+PcaMGRNNmjQp8/wBAwZEUVFRnHXWWVFSUhKXXnrpRqsVAAAAAAAgYgMDmI1lyJAhcdJJJ0WXLl1i1apVMWbMmBg3blz84he/iBUrVkSfPn1i4sSJMXLkyFi1alXMnTs3Ir5fGq1WrVo54/Xr1y+KiopiwIABkSRJXHbZZZv6IQEAAAAAAFuQcgUwI0aM2KCL9O/fv0L927ZtG0OHDo3p06fH4sWLY+zYsXH22WfH4MGDY86cOfHUU09FRESXLl2yzhszZkx069Yt75hnnHFGFBUVRb9+/aKkpCQuv/zyQh4KAAAAAADAepUrgBk4cGBkMpmCL1LRAGbIkCExZMiQ0mvfddddpcfat28fSZKsd4x8ffr27Rt9+/atUC0AAAAAAAAVVaElyMoTfKyWyWQiSZINCm4AAAAAAACqo6LydqxI+FJI/7KsOfsFAAAAAACgOijXDJgxY8Zs7DoAAAAAAAA2G+UKYLp27bqx6wAAAAAAANhslHsJMgAAAAAAAMqnXDNg1uWDDz6IadOmxaJFi8rc96V///4behkAAAAAAIBqo+AAZsKECfFf//VfMXXq1PX2FcAAAAAAAABbkoICmBkzZkTPnj1jyZIlZc56WS2TyRRUGAAAAAAAQHVV0B4wf/zjH2Px4sWl9zOZTE7QIngBAAAAAAC2VAUFMGPGjImI70OWv/zlL6WzYLp27Rr3339/7LnnnpHJZOKXv/xlvPTSS+lVCwAAAAAAUA0UFMDMnDkzMplM7LHHHjFo0KDS9mbNmsVpp50WL774YjRs2DBuvPHGqFevXmrFAgAAAAAAVAcFBTDfffddRES0adPm+0GKvh9m2bJlERHRpEmTOOCAA2LZsmVx9dVXp1EnAAAAAABAtVFQANO4cePvT/7/g5fVs1ymTJlS2ufzzz+PiIgJEyZsSH0AAAAAAADVTs1CTmrSpEl89dVXMW/evIiIaNeuXUyePDlmzJgRJ554Ymy99dbxzjvvRETE0qVLUysWAAAAAACgOigogNltt93iww8/jFmzZkVExKGHHhqTJ0+OiIiRI0eW9stkMrHXXnulUCYAAAAAAED1UdASZPvss09ERBQXF8dHH30UgwcPjlq1auXte+WVVxZeHQAAAAAAQDVUUABz0UUXxfTp02PatGnRunXr2G233eLJJ5+MDh06RJIkkSRJtG3bNu6999444YQT0q4ZAAAAAACgSitoCbL69etH/fr1s9qOPvromDp1asyfPz9WrFgRzZs3T6VAAAAAAACA6qagAGZdttlmm7SHBAAAAAAAqFZSCWBWrFgRzz77bHz44YdRs2bN2HXXXeOII44oc18YAAAAAACAzVm5ApgpU6bEo48+GhER7dq1iwEDBpQe++ijj+LYY4+NTz75JOuc1q1bx3333ReHHnpoiuUCAAAAAABUfUXl6fT444/Hr371q7jmmmti3rx5Wcd+/OMfx8cffxxJkkSSJBERkSRJfPrpp3H88cfHZ599ln7VAAAAAAAAVVi5AphJkyaV/rtPnz6l/37ppZdi4sSJkclkIpPJRESUhjAREd98803ccsstadUKAAAAAABQLZQrgJk6dWpERLRt2zbatm1b2v7EE09k9TvkkEPivvvui9NOO620bfTo0SmUCQAAAAAAUH2Uaw+YefPmRSaTiV122SWr/ZVXXolMJhNJkkRRUVHce++90bZt2/jRj34UL7/8chQXF8fHH3+8UQoHAAAAAACoqso1A2bBggUREVG7du3StmXLlsXkyZNL73fp0qV0dkxRUVHstddeERHx7bffplUrAAAAAABAtVCuAGarrbaKiIjZs2eXtr322muxcuXKiIjIZDJx2GGHZZ2zei+YNUMbAAAAAACALUG5Aph27dpFkiTx7rvvxpNPPhnffPNN/P73v4+I/xe0dO3aNeucWbNmRUREixYt0qwXAAAAAACgyivXHjA9evSI999/PyIiTj755NL21fu/1KtXL4466qjS9q+++iqmTp0amUwmOnTokHLJAAAAAAAAVVu5ZsAMHTo06tWrFxHfz3hZPesl4vsQ5vzzzy89HhFx//33l/Y54IAD0qwXAAAAAACgyitXANO+fft47LHHomnTpqVtq4OYE044IX7zm9+Utq9atSpuvvnm0vs9e/ZMsVwAAAAAAICqr1xLkEVEHHXUUfHJJ5/Ec889Fx999FFstdVWcfDBB8eBBx6Y1W/+/Pnxi1/8IiK+nx1z8MEHp1sxAAAAAABAFVfuACYiol69enHKKaess0/Tpk1jwIABG1QUAAAAAABAdVauJcgAAAAAAAAoPwEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACmrWchJ48ePj4iIZs2axW677ZZqQQAAAAAAANVdQTNgunXrFt27d4+rr766zD4DBw6MbbfdNpo0aVJwcQAAAAAAANVRQTNgymPx4sWxYMGCyGQyG+sSAAAAAAAAVdJG2wNm/vz5G2toAAAAAACAKq3cM2B+/etf57S9//77eduLi4tj3Lhx31+g5kabZAMAAAAAAFAllTsd+dWvfpW1nFiSJPHBBx/ENddck7d/kiQREdGmTZsNLBEAAAAAAKB62WjTU1aHNaeeeurGugQAAAAAAECVVKEAZvWslrLur6lRo0Zx5plnljlDBgAAAAAAYHNV7gBmxowZEfF96LLjjjtGJpOJY489Nv785z9n9ctkMlG3bt1o1qxZupUCAAAAAABUE+UOYNq1a5d1P0mS2HrrrXPaAQAAAAAAtnQF7QFTUlKSdh0AAAAAAACbjYICmDW9/vrr8eyzz8asWbPiu+++iwcffDCKi4tj5cqVUaNGjWjdunUadQIAAAAAAFQbBQcw8+fPj379+sWzzz4bEd8vSZbJZCIi4rLLLosHHnggMplMfPjhh7HTTjulUy0AAAAAAEA1UFTISStWrIhjjz02nn322UiSJJIkyTr+k5/8pLT94YcfTqVQAAAAAACA6qKgAOZvf/tbvPnmm2Ue79q1a9SvXz8iIsaOHVtQYQAAAAAAANVVQQHMfffdV/rv3/3ud9GzZ8+s4zVq1IjOnTtHkiTxwQcfbFiFAAAAAAAA1UxBAcyUKVMik8lEly5d4tJLL40GDRrk9GnWrFlERHzxxRcbViEAAAAAAEA1U1AAs2TJkoiIaN26dZl9FixYEBERmUymkEsAAAAAAABUWwUFME2bNo0kSWLKlCl5j3/99dfx9ttvRyaTKZ0JAwAAAAAAsKUoKIDZd999IyJi5syZMXjw4Jg/f37psVdeeSVOOOGE0lky++23XwplAgAAAAAAVB81Czmpf//+8fTTT0dExK233lraniRJdO3aNatvv379NqA8AAAAAACA6qegGTCnnHJKHHPMMZEkSWlbJpOJTCaT1Xb00UfHiSeeuOFVAgAAAAAAVCMFBTAREY8++mj07ds3kiTJukV8PxPm1FNPjYcffji1QgEAAAAAAKqLgpYgi4ioW7du3HvvvXHllVfGM888EzNnzoyIiHbt2sWxxx4bnTt3TqtGAAAAAACAaqXgAGa1Tp06RadOndKoBQAAAAAAYLNQ8BJkAAAAAAAA5FeuGTA9evQo+AKZTCZefPHFgs8HAAAAAACobsoVwIwdOzYymUyFB0+SpKDzAAAAAAAAqjNLkAEAAAAAAKSsXDNg2rZtayYLAAAAAABAOZUrgJk5c+ZGLgMAAAAAAGDzYQkyAAAAAACAlJVrBszadtxxx4iIOO644+KWW27J22fEiBHxzjvvRETETTfdVFh1AAAAAAAA1VBBAczMmTMjk8nEF198UWafp59+Oh599NHIZDICGAAAAAAAYIuy0ZYgW7FixcYaGgAAAAAAoEor9wyY2bNn57QtWbIkb3txcXG88cYbERGRyWQ2oDwAAAAAAIDqp9wBTPv27bPClCRJ4tlnn40ddthhnec1bty44OIAAAAAAACqowrvAZMkSd5/ry2TyUQmk4lDDz20sMoAAAAAAACqqQrtAbOuwCVf3+bNm8f1119f4aIAAAAAAACqs3LPgLn66qtL/33NNddEJpOJ3XbbLU499dSsfplMJurWrRs777xzHH300bH11lunVy0AAAAAAEA1UHAAkyRJ7L777lntAAAAAAAAFLAHTETE8OHDIyKiffv2adYCAAAAAACwWSgogBkwYEDadQAAAAAAAGw2Cgpgzj777HL3zWQycccddxRyGQAAAAAAgGqpoADmrrvuikwms95+SZIIYAAAAAAAgC1OQQHM+iRJsjGGBQAAAAAAqBYKDmDWFbKsnh2TJIkwBgAAAAAA2OIUFXJSSUlJ3tvcuXPjsccei1122SUiIs4999woKSlJtWAAAAAAAICqrqAApizNmzePk046KZ5//vnIZDJx++23x3333ZfmJQAAAAAAAKq8VAOY1dq0aRPbb799JEkSw4YN2xiXAAAAAAAAqLI2SgAzYcKE+PTTTyMiYsqUKRvjEgAAAAAAAFVWzUJO6tGjR972VatWxfz58+ODDz6IJEkiIqJ27dqFVwcAAAAAAFANFRTAjB07NjKZTN5jq4OXTCYTmUymzLAGAAAAAABgc5X6EmSrg5kkSaJp06bx29/+Nu1LAAAAAAAAVGkFzYBp27ZtmTNgatWqFS1btoxu3brFBRdcEM2aNdugAgEAAAAAAKqbggKYmTNnplwGAAAAAADA5iP1JcgAAAAAAAC2dAXNgImIeO+992LixInx5ZdfRkREs2bN4gc/+EF07tw5teIAAAAAAACqowoHMLfddlvceOONMXv27LzH27ZtG//93/8dgwYN2uDiAAAAAAAAqqNyL0G2fPnyOPHEE+P888+PWbNmRZIkeW+zZs2Kn/3sZ3HiiSfGihUrNmbtAAAAAAAAVVK5A5jzzz8/nn766UiSJDKZTGQymZw+q9uTJImRI0fG+eefn2qxAAAAAAAA1UG5liCbNGlS3HHHHaWhS5Ikseuuu8ZBBx0ULVq0iIiIzz//PCZMmBBTp04tDWHuuOOO+NnPfhZdunTZaA8AAAAAAACgqilXAHPHHXeU/nu77baLESNGRI8ePfL2HTNmTPTv3z8+++yziIj4+9//HrfccksKpQIAAAAAAFQP5VqCbNy4cd93LiqKZ599tszwJSKie/fu8cwzz0RR0fdDjx8/PoUyAQAAAAAAqo9yBTCfffZZZDKZ2G+//aJz587r7d+5c+fYf//9I0mS+PTTTze4SAAAAAAAgOqkXAHM4sWLIyKiWbNm5R64adOmERGxZMmSAsoCAAAAAACovsoVwGy77baRJEm899575R548uTJERGxzTbbFFYZAAAAAABANVWuAGa33XaLiIhZs2bF//3f/623/x//+MeYOXNmZDKZ2H333TesQgAAAAAAgGqmXAHM0UcfXfrvSy+9NAYOHBhvvfVWlJSUlLaXlJTE22+/HWeddVYMHTq0tP2YY45JsVwAAAAAAICqr2Z5Op177rnx29/+NhYtWhRJksTdd98dd999d9SqVSuaNGkSERHz5s2L5cuXR0REkiQREdGoUaM455xzNlLpAAAAAAAAVVO5ZsA0atQo/v73v0dERCaTiYjvQ5Zly5ZFcXFxFBcXx7Jly0qDl4iIoqKi+Pvf/x6NGjXaCGUDAAAAAABUXeUKYCIiTjnllLjvvvti6623jiRJIpPJ5L0lSRL16tWL++67L04++eSNWTsAAAAAAECVVO4AJiLitNNOi48//jiuvPLK6Ny5c0R8PxNm9cyXzp07x//8z//Exx9/HD/60Y/SrxYAAAAAAKAaKNceMGtq3rx5XHvttXHttdfGypUr4+uvv46IiG233TZq1qzwcAAAAAAAAJudDUpMatasGc2bN0+rFgAAAAAAgM1ChZYgAwAAAAAAYP0EMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQsoIDmGnTpsXZZ58dO+64Y9StWzdq1KiR91azZs006wUAAAAAAKjyCkpH/vWvf0W3bt1iyZIlkSRJ2jUBAAAAAABUawXNgLniiiti8eLFERGRyWRSLQgAAAAAAKC6K2gGzIQJEyKTyUSSJNGmTZvYf//9o379+mnXBgAAAAAAUC0VFMCs3telY8eO8e6770atWrVSLQoAAAAAAKA6K2gJssMPPzySJImWLVsKXwAAAAAAANZSUABz3XXXRd26dWPChAnx+OOPp10TAAAAAABAtVbQEmR77LFHvPDCC9GzZ8/o06dP7LLLLrHbbrtFo0aNcvpmMpm44447NrhQAAAAAACA6qKgAGbp0qVx7bXXxnfffRcREVOnTo0PP/wwp1+SJAIYAAAAAABgi1NQAHPVVVfF888/H5lMJu16AAAAAAAAqr2CApgHHnigNHxJkiTVggAAAAAAAKq7okJO+vrrryMiolGjRjF69OhYuHBhrFq1KkpKSnJuq1atSrVgAAAAAACAqq6gAGb33XePiIj9998/jjjiiGjQoIHlyAAAAAAAAP5/BQUwl112WSRJEpMmTYp58+alXRMAAAAAAEC1VtAeMC1btoxevXrFqFGjYr/99otBgwbF7rvvHo0aNcrb//DDD9+gIgEAAAAAAKqTggKYbt26lS45NnPmzLjyyivL7JvJZGLlypWFVQcAAAAAAFANFRTArLbmvi9JkmxwMQAAAAAAAJuDggMYgQsAAAAAAEB+BQUww4cPT7sOAAAAAACAzUZBAcyAAQPSrqNMAwcOjLvuumuTXQ8AAAAAAGBDFVV2AYV47LHHomfPntGkSZPIZDLxzjvv5PRp3759DBs2rPR+kiRx6aWXRsOGDWPs2LGbrFYAAAAAAGDLs0EBTElJSTzwwAMxYMCA6NatWxxwwAEREfHWW2/F+PHj49VXXy1o3K+++ioGDBgQbdu2jfvvvz923nnnOPXUU2P58uUREbF48eI49NBD48YbbyzXeKtWrYqf/OQnMWLEiBgzZkx069atoLoAAAAAAADKo6AlyCIiZs2aFSeeeGK89957EfH9DJNMJhMREXfeeWf89a9/jYiISZMmxZ577lmhsYcMGRJvvvlm3H333TFs2LC48MIL47nnnouSkpKIiOjXr19ERMycOXO9Yy1btiz69u0bb7/9drz88suxyy67VKgWAAAAAACAiipoBsy3334bPXv2jHfffTfv8QEDBkSSJBER8eijj1Z4/EmTJkX//v2ja9eu0ahRo+jevXvceOONUadOnQrX2atXr3j//ffj1VdfXW/4smzZsli0aFHWDQAAAAAAoKIKmgHzpz/9KaZPnx6ZTKY0aFnTgQceGNtss00sWLAgXn755QqPf8ghh8Tw4cNjr732KqS8Ur/5zW+iQYMG8cEHH0SzZs3W2/+GG26Ia665ZoOuCQAAAAAAUNAMmMceeywiIjKZTDz00ENx0kkn5fTZY489IkmS+PDDDys8/k033RSnnXZaDBkyJEaMGBFdunSJ2267rcLj9OzZMxYvXhzXX399ufpfccUVsXDhwtLbnDlzKnxNAAAAAACAggKYadOmRSaTiQMOOCD69OkTNWrUyOmzzTbbRETEvHnzKjx+vXr14rrrrovp06dH796947zzzouhQ4eW7itTXkcccUQ8+eSTcdttt8VFF1203v61a9eOhg0bZt0AAAAAAAAqqqAAZvny5RHx/0KWfL788suIiKhZs6BVzko1btw4Bg0aFMcee2xBy5n17Nkznn766fjb3/4WF1544QbVAgAAAAAAUB4FBTDNmzePJEli0qRJsWrVqpzjs2fPjrfeeisymUy0bNmywuMPGTIkxo0bFwsXLoxVq1bFmDFjYty4cbHPPvtERMTXX38d77zzTrz//vsREfHhhx/GO++8E3Pnzs073pFHHhkjR46MO+64Iy644IIK1wMAAAAAAFARBQUwBx10UEREzJ07N/r06ROfffZZ6bF77rknjjrqqFi5cmVW34po27ZtDB06NNq0aRP33Xdf9O/fP84+++wYPHhwREQ89dRTsffee0evXr0iIuLHP/5x7L333uvcJ6ZHjx4xatSouOuuu+L888+PJEkqXBcAAAAAAEB5FLQ+2H/913/FQw89FBHfhyGrJUkSAwYMyAo3fvKTn1R4/CFDhsSQIUMiImLgwIFx1113ZR0fOHBgDBw4cJ1jzJw5M6etW7du8e2331a4HgAAAAAAgIooaAbMkUceGQMHDswKWjKZTGQymUiSJDKZTERE9O/fP7p165ZKoQAAAAAAANVFQQFMRMTf//73uOKKK6J27dqR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cubMqV69eunx48f69ttvderUKZUpU8blf6m/CbgGxvH29pbFYvlHr71z546D06Bx48ZavXq1fHx81KFDBwUHByt79uxGx3JpYWFhts9v376tUaNGqU6dOipXrpwkKSIiQmvXrtWQIUMUEhJiVExTqVSpkt5//321bdtW165dU968eVWwYEGdPn1aPXr00NChQ42O6PLc3d119epVZcqUyW789u3bypQpk+Li4gxK5vo8PT118uRJ5cqVS++9954KFiyoYcOGKSoqSnnz5rXbngyO4eXlpWPHjil37twaPny4jh07pqVLl+rgwYOqV6+erl27ZnREU+D3kLH8/f317bffqkaNGkqVKpUOHz4sPz8/nTx5UuXKlePvMyd4+XdRUFCQKlSooAEDBujixYvKmzevHj16ZHREl/fyc4p27dopW7ZsGjdunC5evKgCBQooOjra6Igur3Dhwvr8889Vr149HT16VKVKlVKfPn20efNm5cuXT3PnzjU6osOwAsaFZM6cWcePH1dcXJzWrFmjWrVqSZJiY2Pl7u5ucDrXFhkZafsYPXq03n77bZ04cUJ37tzRnTt3dOLECRUvXlyfffaZ0VFdnre3t6KioiQ9r67XrFlTkmS1WrmxdxKugXGmTJmiyZMna/LkyRo8eLAkqU6dOho+fLiGDx+uOnXqSHq+3QAcb/ny5bp8+bI++OADLV68WLlz51ZgYKCWLl2qp0+fGh3PJQUHB9s+du7cqZEjR2rhwoXq2bOnevbsqYULF2rkyJHaunWr0VFNgxmfxrNara8szt++fVspU6Y0IJF5+Pv7a/ny5YqKitLatWtVu3ZtSdKNGzeUOnVqg9OZg4eHh63QtWHDBts1SJcuHRMUnYjfQ8a6fPmy/P39E4zHx8dzT+okBQsW1IwZM7R9+3atX79edevWlSRduXJF6dOnNzidOeTMmVMRERGKiYnRmjVrbO8Hd+/eVfLkyQ1OZw6RkZEqUKCAJGnZsmVq0KCBxowZo+nTp+vXX381OJ1jJTE6ABJPhw4d9N577ylr1qyyWCy2h5579uxRvnz5DE5nHkOGDNHSpUuVN29e21jevHk1efJkvfPOO2rdurWB6Vxf06ZN1apVKwUEBOj27dsKDAyUJB06dOiVN51IfFwD4wQHB9s+b9asmUaOHKmPPvrINtazZ09NmzZNGzZsYPa/k2TM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\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**The top five highest product sales are as follows:**"
      ],
      "metadata": {
        "id": "Y3jRlIkB84U-"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "- Starchy Foods:$2374.33 \n",
        "\n",
        "- Seafood: $2326.07\n",
        "\n",
        "- Fruits and Vegetables: $2289.00\n",
        "\n",
        "- Snack Foods: $2277.32\n",
        "\n",
        "- Household: $2258.78\n"
      ],
      "metadata": {
        "id": "neOi6eEW-QYc"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "**The bottom five least product sales are as follows:**\n",
        "\n",
        "\n"
      ],
      "metadata": {
        "id": "cKPD3Kvf_LjB"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "- Frozen Foods:             $2132.87    \n",
        "\n",
        "- Breakfast:             $2111.80 \n",
        "\n",
        "- Health and Hygiene:       $2010.00 \n",
        "\n",
        "- Soft Drinks:              $2006.51 \n",
        "\n",
        "- Baking Goods:  $1952.97 "
      ],
      "metadata": {
        "id": "3KxLqEkj_aVV"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Which Outlet Location had the least and most amount of Item sales**"
      ],
      "metadata": {
        "id": "EJKJFUVZBGx9"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "means2 = df.groupby('Outlet_Location_Type')['Item_Outlet_Sales'].mean().sort_values(ascending=False)\n",
        "means2"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "gyuA0x4n_rh-",
        "outputId": "48017201-4d0d-483d-c87a-042d8545a615"
      },
      "execution_count": 38,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Outlet_Location_Type\n",
              "Tier 2    2323.990559\n",
              "Tier 3    2279.627651\n",
              "Tier 1    1876.909159\n",
              "Name: Item_Outlet_Sales, dtype: float64"
            ]
          },
          "metadata": {},
          "execution_count": 38
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "fig, ax = plt.subplots(figsize=(20,10))\n",
        "\n",
        "ax = sns.barplot(data=df,x='Outlet_Location_Type', y = 'Item_Outlet_Sales', order = means2.index, ci = None)\n",
        "plt.xticks(rotation = 90)\n",
        "ax.set_title('Average Outlet Sales Based on Location', fontsize = 20, fontweight = 'bold');\n",
        "ax.set_xlabel('Outlet Location Type', fontsize = 15, fontweight = 'bold')\n",
        "ax.set_ylabel('Item Outlet Sales', fontsize = 15, fontweight = 'bold');\n",
        "\n",
        "def hundred_k(x,pos):\n",
        "  \"\"\"function for use with matplotlib FuncFormatter - formats money in millions\"\"\"\n",
        "  return f'${x*1e-3:,.0f}K'\n",
        "\n",
        "price_fmt_100k = FuncFormatter(hundred_k)\n",
        "\n",
        "ax.yaxis.set_major_formatter(price_fmt_100k)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 711
        },
        "id": "1aMbyTcoBkcx",
        "outputId": "20b894ae-93de-4cf8-86e1-45ea4070ddab"
      },
      "execution_count": 39,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "<ipython-input-39-a111e082dfc4>:3: FutureWarning: \n",
            "\n",
            "The `ci` parameter is deprecated. Use `errorbar=None` for the same effect.\n",
            "\n",
            "  ax = sns.barplot(data=df,x='Outlet_Location_Type', y = 'Item_Outlet_Sales', order = means2.index, ci = None)\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 2000x1000 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAABmAAAAN/CAYAAADJRReoAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/bCgiHAAAACXBIWXMAAA9hAAAPYQGoP6dpAACGcklEQVR4nOzdd7hU1dk/7mcAKYJAaAJK0QiIlYjYUCkqClgwWGISamKJhijYoq81kRhLFI3GkiioscSCRrESBQI2UCCKCoqhqEhUpEhHzv79wY/zZZg5MOewEY7c93XN9XrWXnvtZzYzc/LO56y1MkmSJAEAAAAAAEBqKmzpAgAAAAAAAL5vBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQMgEMAAAAAABAygQwAAAAAAAAKRPAAAAAAAAApEwAAwAAAAAAkDIBDAAAAAAAQMoEMAAAsJl07NgxMplM1mPmzJlbuqzvnb59++bc59GjR2/psthKXXXVVTmvl2HDhm3psvge8RoDANYSwAAA34lBgwblfBmRyWSiWrVqsWjRoi1dHluJqVOnxuDBg+PII4+MXXbZJXbYYYeoVq1a7LzzznHggQfGhRde6Iv1rcC0adPit7/9bbRv3z7q168flStXjho1akTTpk1j3333jW7dusWll14ajz32WHz88cdbutxyrXnz5nk/O9c+qlSpEnXq1Im99torTjrppLj77rtj4cKFW7psypF8AWbfvn23dFkAAN8LAhgAYLMrKiqKRx55JO+x5cuXx/Dhw7/jitjafP755/GTn/wk9txzz7jsssvi5ZdfjpkzZ8bixYtj+fLl8dlnn8X48ePjxhtvjE6dOsVBBx0Ub7zxxndaY3mZZZHvC/u0JEkSv/3tb2OPPfaI6667Ll577bX46quvYtWqVbFkyZL45JNP4p133onnn38+rr322jjllFPiwAMPTO365Fq5cmXMnz8/3nvvvXjiiSfizDPPjF133TX++c9/bunS4Hth2LBhOZ+pV1111ZYuCwAoJypt6QIAgO+/V155JT7//PMSj//973/317bbsAkTJsQJJ5ywwdfI+t58883o0KFD3HHHHdG/f//NWB3rOv/88+Pmm2/e0mWwEV9//XX07Nkzxo8fH/vtt9+WLge2OQcddFCce+65WW177LHHFqoGANiSBDAAwGb34IMPbvD4qFGjYs6cOdG4cePvqCK2FrNmzYpu3brFV199lXOsRYsWcfDBB0eVKlViypQp8cYbb0SSJMXHV65cGb/85S9jxx13jO7du3+XZW+T3nvvvbjlllty2ps1axYHHXRQ1K1bN5YtWxazZ8+OSZMmxddff70Fqtw2HHXUUcVf5i5dujTGjRsXH3zwQVaf1atXx3XXXRf/+Mc/tkSJsE075phj4phjjtnSZQAAWwEBDACwWS1btmyjS4wVFRXFww8/HOeff/53VBVbi5///Oc54UvlypXj7rvvjj59+mS1T5gwIXr27BmffPJJcVuSJPHzn/88Pv7446hTp853UvO26tFHH42ioqKstssvvzyuvvrqnGXOkiSJt99+O5544ol4+umnv8sytwk//elPs2YNrl69Oo4//vh47rnnsvqNHTv2O64MAABYlz1gAIDN6plnnolFixZltZ100klRoUL2/wzJN0umqKgomjZtmrXuev369ePbb78t8XotWrTI6l+jRo1YvHhx3rEff/zx6NWrV7Rq1Spq164dVapUicaNG8cxxxwTt99+eyxbtqzE68ycOTNnTfiOHTtGxJqN5Nfuw1C1atXIZDIxc+bMiFgza+PFF1+MwYMHx4knnhj77rtvNGnSJKpXrx6VK1eOevXqxf777x9nnXVWjBkzpsTrr2vOnDlx7rnnxg9/+MOoWrVq7LjjjtGtW7fiL77Lsn79m2++GQMGDIgf/ehHxZus169fPw466KC44oorYs6cOQXVtiH/+te/Yty4cTntf/7zn3PCl4iIdu3axXPPPRfbbbddVvuCBQtiyJAhOf3Xf87NmzfPW8fG9nZZ23bfffflnNupU6dU94VZuXJlDBs2LE455ZTYddddo2bNmlG1atVo0qRJ9OjRI+6///68r//Ro0cXX3/WrFk5x/Nt3l5a7777bk7bwIED846VyWRi//33j2uvvTbeeeedEp9r2u+FQm3q+z9iTejxyCOPxMknnxytWrWKHXbYISpVqhR169aNVq1aRceOHWPAgAHx4IMPxty5c1Otf30VK1aMfv365bTPmzcvb/+PP/44hg4dGuecc060b98+WrZsGfXq1Yvtttsudthhh2jWrFkce+yxMWTIkBLHWCvN+/D+++/HhRdeGAceeGDsuOOOUbly5ahTp0786Ec/igsuuCA++uijgu7HBx98EP37948mTZpElSpVYuedd46f/exnMWHChILOL6sFCxbEzTffHN26dYsmTZrE9ttvH9tvv300adIkjj322Lj11lvjm2++KfH8Df1e+eyzz2LQoEHRqlWr2H777aN27drRoUOHePjhhzfrcyqLZcuWxd133x0nnnhiNG/ePGrUqBFVq1aNxo0bx5FHHhnXXnttfPnllwWPN3LkyDjrrLNi3333jXr16kXlypWjQYMGsc8++0S/fv3ikUceiSVLlmSds3jx4njmmWfiiiuuiG7dusXee+8dO+20U1SrVi2qVq0aDRo0iEMOOSTOO++8mDhxYt7rduzYMTKZTN731trguaTfq1dddVXO8WHDhpX4HJMkiREjRkS/fv1i9913j9q1axc/zwMOOCAuuOCCeO+99zZ4n9bWu+5j5syZsXr16rj77rvj8MMPj7p160a1atWiVatW8dvf/jYWLFiwwTEBgBQkAACb0fHHH59ERNZj1KhRyeGHH57T/v777+ecf+mll+b0e+GFF/Je66233srp26dPn5x+kyZNSnbfffecvus/GjdunIwZMybvtWbMmJHTv0OHDsnDDz+cVK1aNefYjBkzkiRJknfffXej1133ceyxxyYLFiwo8f6OGTMmqVWrVonn9+vXL7nnnnty2q+88sq843355ZfJscceu9G6qlWrlvz5z38usa5C/OQnP8kZt3Xr1klRUdEGzzvzzDNzzmvUqFFOv/X7NGvWLO94ffr0yfsaLWmcjT3WPbdDhw4lvhbW9/LLLyc77bTTRsdv1apVMmXKlKxzR40aVeo6S+uoo47a4HMtrTTfCxv7N1xXGu//r776KjnggAMKrv0Xv/hFme9Ts2bNcsYbOnRoTr/HH388p1/Tpk3zjtmzZ8+Ca69du3by+OOPb9b7sGTJkqRfv35JJpPZ4PmVKlVKLr300mT16tUl3q8HH3wwqVKlSt7zK1asmNxwww3JlVdeWdA9LY2hQ4cmO+ywQ0H385FHHsk7Rkm/V4YPH77Bsc8999xNqj3f+yff785CPPvss0mDBg02eh+qVauWDBkyZINjTZs2LWnXrl1Br6313+/PPPNMqT5f+vfvn6xcuTJrjHyf3xt6rPt7tTSvsQ8//DDZb7/9Njp+JpNJ+vXrlyxZsiTvOPnqff3115P999+/xDFbtmyZzJ07d6P/rgBA2ZkBAwBsNl9//XU8//zzWW077rhjHH744dGzZ8+c/n//+99z2vLNhChpT4N87esu0xMR8frrr8chhxwSU6dO3VDpEbFmZsmRRx4ZL7/88kb7RqyZ+dK7d+9Yvnx5Qf0LMWLEiPjZz36W99j06dOje/fusXDhwhLPHzp0aFxzzTUFXWvevHlx8MEHx4gRIzbad9myZTFgwID4/e9/X9DY+YwaNSqn7dRTT93o7IzTTjstp+3zzz+PadOmlbmWLW348OFx1FFHxWeffbbRvtOmTYtDDjlko38Nnbb69evntHXv3j3OPvvsGDFixEZnSmyqDb0XCpXW+3/QoEExfvz4TaolTUVFRXH//ffntHft2nWTx16wYEGcdtpp8frrr+ccS+M+LF++PI444ogYOnRo1h5P+Xz77bfxhz/8IU4//fS8x8eMGRO9e/eOFStW5D2+evXquPDCC+Oxxx7bpJrXd91110W/fv02OLtlrQULFsRPfvKT+Mtf/lLQ2O+++26cfPLJGxz7lltuiZdeeqngejeXhx9+OI499tj44osvNtp32bJlcd5558XFF1+c9/jkyZNj//333+yzlta69957Y+DAgd/JtdY1derUOOCAA0qchbOuJEli6NChcfTRR8fKlSsLGv+EE06It956q8TjH3744RZ53gCwLbEHDACw2Tz66KOxatWqrLYTTzwxKlSoED179ozzzjsv6wu3hx56KK655pqsL+BbtmwZBx98cNaXf08++WTceeedUbly5Zzrrat58+bRoUOH4p8XL14cP/7xj3OWFmrUqFF06NAhqlevHhMmTMhaMmnVqlVx2mmnxYcffhi1a9fe4PP93//+FxERlSpVis6dO8euu+4aX375Zd6goWrVqtGmTZuoX79+1KtXL2rUqBHffPNNvP/++zFhwoSs+/Lss8/Gv//97zj88MOzxjjnnHPyLq92xBFHRIsWLeLdd9+NV199NWbMmLHButfq27dvTJ8+PautWrVq0aVLl2jYsGFMnz49XnnllazarrzyyujYsWMcdthhBV1jrc8//7z4fq2rXbt2Gz23bdu2kclkcr6snTRpUrRq1apUdRTi3HPPjYiIl156KWej8549e8bOO++c1bb+zxsze/bs6NWrV87+KrvuumsccsghUalSpRg3blzWv82iRYvipJNOinfffTcqVaoUO++8c3Gd9957b86XtWuPbYrOnTvHQw89lNW2dOnSuOOOO+KOO+6IiIhddtklDjnkkOjcuXOceOKJ8YMf/GCj46bxXihEWu//VatW5XyBX6VKlTjiiCOiadOmsXr16pg7d25MmTKl4PdeaT300EMxefLkiFjzb/Daa6/lBHI777xzXH755Rscp3nz5rHbbrtF3bp1o27dupEkSXz++efx6quvZi0RtWrVqvi///u/eOWVV7La0rgPF154YbzxxhtZbZUqVYojjzwymjVrFp9++mm8+OKLWUvv3XvvvdG5c+esQK6oqChOP/30WL16dc5Y3bp1i8aNG8eECRPi7bffjvfff3+D96U0Xn311bjkkkty2ps0aRJHHXVUFBUVxYsvvhiff/551vFzzz03Dj300Nhnn302OP7XX38dERH16tWL7t27x8qVK2P48OE5IdNtt90WXbp02cRnU3YzZsyI/v3753wu16tXL7p27RpVq1aNV155JT7++OOs49dff3106NAhunXrVty2dOnSOP744/OGTmv/N8H2228fs2bNirFjx24wnKpRo0a0adMm6tWrV7z81sKFC+M///lPzvKId955ZwwaNCh23XXXiFizXGqbNm3i/fffj5EjR2b1PfDAA+Oggw7Kalv/541ZvXp19OzZM2cZsCpVqsQxxxwTjRo1iv/85z854ee4cePi8ssvj+uuu26j1/jiiy+iQoUKcfTRR0fTpk3jpZdeynk/PvbYYzFkyJBo0KBBqeoHAAq0xebeAADfe4ceemjOchf/+te/io8ffPDBOcfHjh2bM85dd92V0+/pp5/O6vP666/n9Lniiiuy+lx33XU5fX72s58ly5cvz+p3xRVX5PS7+uqrs/rkWyomIpK6desmb7/9dlbfFStWFC9tMm/evOTFF19Mli5dWuJ9Gz58eM645513Xlaf9957L+/111/i5Prrr9/oUilJkiRvvPFGTp8999wzZ2mSF198MalUqVJWv06dOpX4XEryzjvv5K3rnXfeKej8H/zgBznnrr8k2vrHy7oEWWn7rauQJch+9atf5fS56KKLspZZWrVqVdKvX7+cfvfdd1/ONfMtWZWGpUuXJrvuuutGl8lZ+6hcuXLyq1/9Kpk/f37e8dJ6LyRJYf82ab3/P/vss5zjI0aMyFv/p59+mtx1113JX/7ylxKf48bk+/fc2OOwww5LZs+eXeKYY8aMSaZPn17i8cWLF+d8PmcymeTrr79O9T588sknyXbbbZc1RuPGjZNp06Zl9Zs8eXLOEly77bZb1nsk35JTlStXzvmdcskllxT02VmofEvzHXPMMcmyZcuK+3zzzTdJ+/btc/qdfPLJWWOV9Htlr732SubNm7fB51q7du0y1Z8k6SxBdvrpp+eM0aZNm6zXzMqVK5OTTz45p1+7du2yxrrhhhty+lSsWDH529/+lrNE5eLFi5M//elPyVtvvZXVPnPmzOTf//53zrJi67rppptyrpNvWbShQ4fm9CtpGc+1ClmC7MEHH8zpU7NmzWTy5MlZ/W6++eacftWqVUu+/PLLrH75ft9UrFgxee6554r7zJs3L2nRokVOvyeffHKDzwcAKDszYACAzWLWrFnx6quvZrXVq1eveEPhiDV/Xbr+X3Y++OCDceihh2a1nXrqqXHuuedmLe31yCOPxHHHHVf88/rLj2UymZzlyx5//PGsn6tUqRK33XZbVKlSJav98ssvjxtuuCHrL+Uff/zxuOKKK0p6usVuuumm2G+//bLa1p2pU6dOneK/Un7//fdj0qRJMWPGjFi8eHGsWLEikiTJuwzP+suTPPfcczl92rZtm7Pk2vnnnx933nln/Pe//91g3U888URO2w033BA77rhjVluXLl2iU6dOWX8NPHr06Jg3b17UrVt3g9dY16JFi/K2b7/99gWdX7169Zg/f35W24aWYttaJUmSc+8bNWoU1157bVSo8P9WC65UqVJcc801MXTo0Ky+jz/+ePTu3fs7qbVatWoxYsSI6N69e0EzO1auXBl33HFHjBw5Ml577bWcJczSei8UKq33/w477JAzdklLx+20005xxhlnlKneTbF2VsZf//rXqFatWs7xtTOIFi9eHK+++mpMnTo15s2bF0uWLCmeQbL+TJIkSWLy5MnRqVOniEjnPjz99NM5syQvu+yyaNmyZVbbvvvuGz/5yU/ir3/9a3Hb9OnTY/LkycWft+svdxmxZlbf+r9Pfve738V9990Xc+bMyVtraSxcuDBrVlDEmt89d955Z1StWrW4rUaNGnH77bdHmzZtsvo+99xzsWrVqthuu+02eJ1rr7026tSpU/zzscceGzvttFPW/V6wYEHMnz+/oFlnaUuSJJ566qmc9ltuuSWrnu222y5uv/32+Oc//5m1hNaECRNizpw50bhx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LJinn346J0hp2bJl3HLLLXHAAQfED37wg8hkMjFt2rTYfffdCxpzc6hfv37O759PPvkk72vi+/raAYBtgQAGAEhVUVFRPPzww5s0xvLly2P48OE5y4j17ds3K4BZtmxZDBw4MGcZjvXPi1gT4Oy3335ZMyy++eabOP/88wtaomv16tWb/Fej+b7ov+iii3KWZsu3L8L62rRpkzNTaOTIkXHeeedltRUVFeXdHHl97dq1izFjxmS1de7cOX72s59t9Nyy3ptf/vKXOWHKe++9F/fee2/ezZYj1mzWfs899+Qda301atTIWtf+66+/Lt43aN3x3nnnnYLqzfcc883gKo1mzZpF/fr1s5aiWbhwYVx77bUlzmha//rr11VSnZv6+h0+fHi0aNEi9t577w32q1q1atSsWTMngFl3E/E03wuF2Jzv/4oVK8a+++6btY/H9OnTo0WLFln93nzzzc0ewORbkmj9JerWv/c1a9aMe+65JypUyF4cobT3viz3oV27dnHfffdlHW/Tpk383//930avt/6/SZs2bXLCp5deeil+/OMfZ7V9++238corrxT2pDZiv/32ywkL3nvvvZg1a1bO0k/vvPNOTkhevXr12GOPPVKpZUtq3rx5zufYN998E+PGjcvZB+arr77Ku/dJu3btImLN66ht27Yxfvz4rON33XVXQYFgRP7Plz/84Q9xzDHHZLUV+hrfHJ/9EWue8/q1Pvfcc9G2bducvs8991ze8wGArZ8lyACAVI0ePbrEmRilkW8ZstatW+d84TB06NCsn5s1axadOnXKO+b6X8QlSRInnXRSifUuWrQoHnvssTj22GPjD3/4Q2nKzyvfTIX//Oc/WT/Pnj27oM2Gu3XrltP2/PPP54QtQ4YMKWi5ofXvTUTEueeeW+Im76tWrYpRo0bFGWecET169Njo+PkcccQRceihh+a0n3POOXn//d96663o2rVrrFy5Mqu9du3aOcFTROT8tXRRUVHcddddxT8vX748Bg0aVHC9NWrUyGl7//33Cz4/n0wmEyeeeGJW2zfffBOnnHJKfP3113nP+eqrr+K+++6LDh065L1Pm6POiIhXXnkl9t133+jWrVs8+uijJe6VcM8998SsWbOy2mrVqhU//OEPi39O871QqDTf//369YtHH300715EEZF3v6jS7rdSWu+++2488cQTOe3NmzfP+nn9e7948eL4+OOPi39OkiT+9Kc/xahRozZ6zU29D8cdd1xUqpT9N4HXXHNNPPPMM3nHKyoqijfeeCPOP//8OPDAA7OO5Vv6btiwYTmhzNVXX53K76iINa/rzp07Z7UlSRJnnXVWLF++vLhtyZIlcc455+Sc361bt9huu+1SqWVLymQyeX8P/OY3v8maAbVq1ao4++yzcz7D27Vrl7XE6PpBbETEjTfemDd8X758edx+++3x9ttvF7cV8vkyZcqUuOiii0p8TuvaXJ+p+X7v/ulPf8r5o4BbbrklJk2alNVWrVq16NKlyybXAABsfmbAAACpyrf82OWXXx6/+93vSjxn8eLFUb9+/awvrEaNGhVz5szJ2felb9++ef96dq1evXqVuHzVr3/967jllluyvhQcP358NG/ePDp06BDNmjWLypUrx9dffx1Tp06NDz74oHi/gv3337/EaxYq31+1PvDAAzF16tTYb7/9Yu7cuTFy5MiCNgFu3bp1dOnSJV566aXitiRJ4sc//nF06dIldt1115gyZUqMHTu2oNoOPvjg6Nq1azz//PPFbfPmzYtDDjkk2rZtG7vvvnvUrl07Fi5cGP/973/jnXfeKd5/oUOHDgVdI5+///3v0bZt26x9WFasWBG9evWKa665Jg466KCoUqVKvPfee/Haa6/lLCuTyWTi73//e9bsirU6dOiQ80XWoEGDYsyYMVG/fv0YOXJkzvJMG7L+X/JHRFx88cUxbty4aNiwYWQymWjUqFGp9za59NJL44EHHshakmvEiBGx8847R4cOHYo34Z43b168//77MW3atOKltPItodWiRYuc53300UfHMcccU7ys32GHHVaqZc7WSpIknn/++Xj++eejUqVK0aZNm9htt92iTp06sXDhwpg4cWLOUnYRa75QXfeL9jTfC4VK8/0/cuTIGDZsWFSqVCn22GOPaNmyZfEyZ7Nmzco7w6Jly5apPZeHHnooJk+eHBFrvtSeMWNGjBw5Mme2S0TE8ccfn/Vz27ZtY/To0cU/FxUVxf777x/dunWL6tWrx/jx4+Pdd98tqI5NvQ9NmzaN008/Pe64447ituXLl8fxxx8frVu3jn333Tfq1q0bixcvjpkzZ8Y777xTPLNq/RkmXbt2jd122y0rcF6xYkV07NgxunXrFo0bN44JEyYUtM9UaVx55ZXxr3/9K+uz6YUXXoiWLVtGly5doqioKF588cWYM2dO1nmVKlWKyy67LNVa0jR+/Pi8wfa6dtttt/j1r38dEWuWrHvggQeyfo9Pnjw5WrRoEd26dYsqVarEK6+8khX2rXXVVVdl/Xz22WfHkCFDsoKy1atXxy9/+cu4/vrro3379lGtWrX49NNPY+zYsTF//vyswDDf58vvf//7GDt2bLRq1Spmz55d4vsln3yf/U8++WR06dIlWrZsWfzZNnjw4KhevXpBY0ZEnHrqqTF48OCsMGfhwoVxwAEHRNeuXaNRo0bxn//8JydEjIgYMGDARpdFBQC2EgkAQEqWL1+e1KpVK4mIrMfkyZM3eu5xxx2Xc96NN96Y02/evHlJlSpVcvqufUyfPn2D13n11VeTqlWrlnh+SY8rr7wya5wZM2bk9OnQocMGr7169epk33333ei1jj766Jy2Zs2a5Yz30UcfJTVq1NjoeHvsscdGn0+SJMkXX3yR/PCHPyz1vdnY896Y8ePHJ40aNSr1dStXrpzcc889JY47ZcqUpFKlShsdp169ejlto0aNyhlv+vTpSSaT2eBYe+65Z9Y5HTp0yOkzY8aMnLEff/zxpEKFCqW+B0OHDs0Z65577tnoeeecc05p/5mSc845p9T1RUTSsGHD5LPPPssaK+33Qp8+fQr6N0zr/b/TTjuV6vxatWoln3/+eanveZIkSbNmzcp03yMi6datW854TzzxxEbPq1SpUtK5c+eNvt7SuA9Lly5NDjzwwFI/t3yvg1deeaWg91GTJk0Kei8V6o9//GOp67/99ttzxinN75VCP1sKke/9U8hj/doeeuihjX5Grv+46KKL8tb01ltvFfT7be1j3ff74sWLk4YNG270nHyfL/nu9+rVqwt6H3755ZfF51x55ZUFvcY++OCDpHbt2qW6Z4ceemiyYsWKnLEKfU0MHTo0p1++/00AAKTDEmQAQGpGjBiRsxTND3/4w6w9AUqy/jJMEfln09SpUyeOO+64vGMceuihWcsc5XPIIYfE66+/HnvuuedGa1qrUaNGBT2HjalQoUI8+uijG9xg+uCDD867yXw+u+22Wzz77LNRq1atEvv8+te/joEDB+a0V6lSJaetfv368cYbb8QJJ5xQ0PUjIrbffvucNf5Lq127dvHWW2/FKaeckrMPRUkOOOCAGD16dPTv37/EPnvuuWfccMMNJR7fbrvt4tZbb43u3bsXdM0f/vCHMWDAgIL6llbPnj1j5MiR0bRp04LP2XXXXfP+ZfbPf/7zzbI3QOvWrYtn0BSqTZs28corr+TMZEv7vVCotN7/Jc2yy6devXrx5JNPRsOGDQs+Jw2nnHJKPPbYYzntP/7xj+OCCy4o8bzKlSvHPffcU9D7Oo37UK1atXj55Zfjl7/8ZcHv/+222y5n6a+IiE6dOsWwYcPyLkG11m9/+9sNfm6UxcUXXxz33ntv7LDDDhvtW7t27Xj44Yfj7LPPTrWGrcFpp50WI0aMKGhz+GrVqsXNN98c1113Xd7ja/eB2W+//UpdR/Xq1ePxxx/f4O/G448/Pm699daCxqtQoULceOONBb8+S2P33Xcv+HlmMpno169fvPjiixt8jQMAWxdLkAEAqckXmBS6zNFxxx2Xs5nxpEmT4oMPPojWrVtn9e3Tp088/vjjOWP07du3oGu1adMm3n333Xj22WfjySefjDfffDPmzJkTixYtiqpVq0a9evWiZcuW0a5duzjqqKPisMMO2+QNzNdq2bJlTJo0Ka6//vr45z//GbNmzYrq1atHy5Yt46c//Wn86le/KtWeAIcffni8//778cc//jFGjBgRc+bMiVq1asX+++8f55xzTnTr1i3v8m/169fPO169evXiqaeeismTJ8cDDzwQr776asyYMSMWLFgQFSpUiNq1axeHakcccUR06dIl7/r4pdW4ceP4xz/+EVOnTo3HH388Ro0aFdOnT4958+bFt99+G3Xq1Imdd945DjvssDj22GNL3Odnfeedd17sueeecdNNN8X48eNj8eLF0ahRozjyyCPj/PPPj9atWxf8uolYsxZ/27ZtY9iwYfGf//wnFixYULwc2Kbq3LlzTJ8+PZ544okYMWJETJgwIf73v//F4sWLY/vtt48GDRrE7rvvHgceeGB06dIlZw+MtSpXrhyjRo2Km266KZ566qn46KOPYvHixTnLt5XWOeecE6effnq8+uqr8dprr8XEiRNj+vTp8dlnn8XixYtj9erVscMOO8TOO+8c++23X/z4xz+O7t27l/jeSfu9UKg03v//+c9/YuTIkfHaa6/F5MmTY+bMmfHll1/GihUromrVqtGgQYPYY4894phjjok+ffqUOrgqrapVq0bNmjWjRYsWcdBBB8VPfvKTDS6beMMNN0SHDh3iz3/+c0yYMCGWLl0aDRs2jI4dO8bAgQNj3333zVkWKp+07kP16tXjr3/9a1x88cUxbNiwGDt2bHz44Ycxf/78SJIkatWqFbvsskvsvffe0alTpzjmmGOibt26ecfq1atXtG3bNq6//vr417/+FV9++WXUqVMnDj744BgwYEB06tSpoOdWWv369YsTTzwx7r333hg5cmS8++67xUsr1qtXL/bZZ584+uijo1+/fgUFNeVVt27dYubMmfHAAw/Ec889F5MmTYqvvvoqVq9eHXXq1InWrVvHEUccEaeffnqJv4fWat26dbz99tvx4osvxhNPPBGvv/568Xu1du3a0ahRo9hvv/3i6KOPzgmd27dvH//5z3/i2muvjRdeeCE+//zzqFmzZuy5557Rr1+/6N27d85eVRty0kknxZgxY2LIkCHxxhtvxBdffFG8ROGmatGiRbz11lvx7LPPxuOPPx5vvPFGfP7557F06dKoXbt2NG/ePA4//PDo379/qcJjAGDrkEk29f8TAwBgq9a2bduYOHFiVtv48eM3yywJAAAAYA1LkAEAlGPnn39+jBs3Lu/shuXLl8eAAQNywpfGjRvn3aQYAAAASI8ZMAAA5Vjz5s1j1qxZseOOO8b+++8fO++8c1SsWDE+++yz+Pe//x3z58/POeeuu+6KM844YwtUCwAAANsOAQwAQDm2NoApVK9eveK+++4r1ebZAAAAQOlZggwAYBtQtWrVuPbaa2Po0KHCFwAAAPgOmAEDAFCOTZ8+PYYPHx5jx46NGTNmxBdffBHz58+P7bffPurVqxf77LNPdOzYMXr16hV16tTZ0uUCAADANkMAAwAAAAAAkLJKW7qArVlRUVHMmTMndthhB0t1AAAAAADANi5Jkvjmm2+icePGUaHChnd5EcBswJw5c6JJkyZbugwAAAAAAGAr8sknn8TOO++8wT4CmA3YYYcdImLNjaxZs+YWrgYAAAAAANiSFi1aFE2aNCnODzZEALMBa5cdq1mzpgAGAAAAAACIiCho25INL1AGAAAAAABAqQlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIY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E02aNImIiOnTp8eyZcsiIuK2226L0aNHR4sWLSJJkpg5c2ZqxQIAAAAAAJQHZQpgFi5cGBERtWrVioiIqVOnFh/bf//9IyKidevWERGxfPnyTSoQAAAAAACgvClTALP99ttHRMQHH3wQERFvvfVWRERUq1YtGjduHBER3377bURE1K5de1NrBAAAAAAAKFfKFMDstttukSRJTJ8+PfbYY4+48cYbI5PJxD777FPcZ/bs2RER0aBBg00qsG/fvpt0PgAAAAAAwHetTAFM9+7di/972rRpsWLFioiIOO644yIiYv78+TF16tScUCYNq1atiosvvjj23nvvqF69ejRu3Dh69+4dc+bMyeqXyWTiqaeeyjrvtNNOi5122immTJmSak0AAAAAAADrKlMAM2jQoGjdunUkSRJJkkRExD777BO//vWvIyLiqaeeKl6C7LDDDiv1+F999VX06dMnmjZtGg8//HDstttucfLJJ8fKlStj6dKlMXHixLj88stj4sSJMXz48Jg2bVocf/zxJY63dOnSOP7442PChAkxbty42GuvvcrwrAEAAAAAAApTqSwn1apVK95666148skn49NPP43ddtstjj322KhcuXJERLRp0yYee+yxiIjo0KFDqccfOHBgjB8/Ph544IEYMmRI/OY3v4kXXnghioqKolatWjFy5Mis/rfddlsccMABMXv27GjatGnWsQULFkT37t1j8eLFMW7cuGjYsGFZnjIAAAAAAEDByhTARERUq1YtfvrTn+Y99qMf/Sh+9KMflbmoSZMmRe/evaNDhw4xdOjQ6NSpU3Tq1KnE/gsXLoxMJhO1a9fOap87d2506NAhatSoEWPGjMk5vr4VK1YUL6cWEbFo0aIyPwcAAAAAAGDbVaYlyNY3Y8aMGD16dDz33HNpDBft27ePoUOHxogRIzbad/ny5XHxxRfHaaedFjVr1sw6du6558bKlStj5MiRGw1fIiKuvfbaqFWrVvGjSZMmZX0KAAAAAADANmyTApjHH388WrduHbvttlscccQRxfuwnHfeedG5c+fo0qVLLF26tNTj3nTTTXHqqafGwIED4/777482bdrEnXfemdNv1apVccopp0SSJHHHHXfkHD/22GPjww8/jLvuuqug615yySWxcOHC4scnn3xS6toBAAAAAADKHMBcc801ceqpp8aHH34YSZIU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Qw89FKeeempx28iRI1MoEwAAAAAAoPwoaA+YefPmRSaTiVatWmW1jxs3LjKZTCRJEhUqVIgHH3wwmjZtGqecckqMHTs25syZEx9//PFmKRwAAAAAAGBrVdAMmAULFkRERJUqVYrbVqxYEVOmTCn+uU2bNsWzYypUqBD77rtvREQsXrw4rVoBAAAAAADKhYICmO222y4iImbPnl3c9tprr8W3334bERGZTCYOO+ywrHPW7gWzbmgDAAAAAACwLSgogGnWrFkkSRLvvPNO/POf/4xvvvkmbrjhhoj4f0FLhw4dss6ZNWtWRETsuOOOadYLAAAAAACw1StoD5jOnTvH+++/HxERP/7xj4vb1+7/Ur169TjqqKOK27/66quYOnVqZDKZaNGiRcolAwAAAAAAbN0KmgEzaNCgqF69ekSsmfGydtZLxJoQ5pxzzik+HhHx8MMPF/c58MAD06wXAAAAAABgq1dQANO8efMYPnx41KtXr7htbRBz3HHHxe9///vi9tWrV8ett95a/HOXLl1SLBcAAAAAAGDrV9ASZBERRx11VPz3v/+NF154IaZPnx7bbbddHHLIIXHQQQdl9Zs/f35cdtllEbFmdswhhxySbsUAAAAAAABbuYIDmIiI6tWrR8+ePTfYp169etGnT59NKgoAAAAAAKA8K2gJMgAAAAAAAAongAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZZXKctK///3viIioX79+tG7dOtWCAAAAAAAAyrsyzYDp2LFjdOrUKa688soS+/Tt2zfq1KkTdevWLXNxAAAAAAAA5VGZZsAUYsmSJbFgwYLIZDKb6xIAAAAAAABbpc22B8z8+fM319AAAAAAAABbtYJnwPzud7/LaXv//ffzts+ZMyfGjBmz5gKVNtskGwAAAAAAgK1SwenIVVddlbWcWJIk8cEHH8TVV1+dt3+SJBER0aRJk00sEQAAAAAAoHzZbNNT1oY1J5988ua6BAAAAAAAwFapVAHM2lktJf28rlq1asXPf/7zEmfIAAAAAAAAfF8VHMDMmDEjItaELrvuumtkMpno2rVr3H777Vn9MplMVKtWLerXr59upQAAAAAAAOVEwQFMs2bNsn5OkiS23377nHYAAAAAAIBtXZn2gCkqKkq7DgAAAAAAgO+NMgUw63rjjTfi+eefj1mzZsWyZcviH//4R8yZMye+/fbbqFixYuy0005p1AkAAAAAAFBulDmAmT9/fvTq1Suef/75iFizJFkmk4mIiAsvvDAeeeSRyGQyMW3atPjhD3+YTrUAAAAAAADlQIWynLRq1aro2rVrPP/885EkSSRJknX8F7/4RXH7Y489lkqhAAAAAAAA5UWZApi//vWvMX78+BKPd+jQIWrUqBEREaNHjy5TYQAAAAAAAOVVmQKYhx56qPi/r7/++ujSpUvW8YoVK8bee+8dSZLEBx98sGkVAgAAAAAAlDNlCmDee++9yGQy0aZNm7jgggtihx12yOlTv379iIj44osvNq1CAAAAAACAcqZMAczSpUsjImKnnXYqsc+CBQsiIiKTyZTlEgAAAAAAAOVWmQKYevXqRZIk8d577+U9/vXXX8dbb70VmUymeCYMAAAAAADAtqJMAcz+++8fEREzZ86MAQMGxPz584uPjRs3Lo477rjiWTLt2rVLoUwAAAAAAIDyo1JZTurdu3c888wzERHxl7/8pbg9SZLo0KFDVt9evXptQnkAAAAAAADlT5lmwPTs2TOOOeaYSJKkuC2TyUQmk8lqO/roo+OEE07Y9CoBAAAAAADKkTIFMBERTzzxRJx22mmRJEnWI2LNTJiTTz45HnvssdQKBQAAAAAAKC/KtARZRES1atXiwQcfjEsvvTSee+65mDlzZkRENGvWLLp27Rp77713WjUCAAAAAACUK2UOYNbac889Y88990yjFgAAAAAAgO+FMi9BBgAAAAAAQH4FzYDp3LlzmS+QyWTi5ZdfLvP5AAAAAAAA5U1BAczo0aMjk8mUevAkScp0HgAAAAAAQHlmCTIAAAAAAICUFTQDpmnTpmayAAAAAAAAFKigAGbmzJmbuQwAAAAAAIDvD0uQAQAAAAAApKygGTDr23XXXSMiolu3bnHbbbfl7XP//ffH5MmTIyLipptuKlt1AAAAAAAA5VCZApiZM2dGJpOJL774osQ+zzzzTDzxxBORyWQEMAAAAAAAwDZlsy1BtmrVqs01NAAAAAAAwFat4Bkws2fPzmlbunRp3vY5c+bEm2++GRERmUxmE8oDAAAAAAAofwoOYJo3b54VpiRJEs8//3zssssuGzyvdu3aZS4OAAAAAACgPCr1HjBJkuT97/VlMpnIZDJx6KGHlq0yAAAAAACAcqpUe8BsKHDJ17dBgwbxhz/8odRFAQAAAAAAlGcFz4C58sori//76quvjkwmE61bt46TTz45q18mk4lq1arFbrvtFkcffXRsv/326VULAAAAAABQDpQ5gEmSJPbYY4+sdgAAAAAAAMqwB0xExNChQyMionnz5mnWAgAAAAAA8L1QpgCmT58+adcBAAAAAADwvVGmAKZ///4F981kMnHPPfeU5TIAAAAAAADlUpkCmGHDhkUmk9lovyRJBDAAAAAAAMA2p0wBzMYkSbI5hgUAAAAAACgXyhzAbChkWTs7JkkSYQwAAAAAALDNqVCWk4qKivI+5s6dG8OHD49WrVpFRMRZZ50VRUVFqRYMAAAAAACwtStTAFOSBg0aRI8ePeLFF1+MTCYTd911Vzz00ENpXgIAAAAAAGCrl2oAs1aTJk1i5513jiRJYsiQIZvjEgAAAAAAAFutzRLAvP766/Hpp59GRMR77723OS4BAAAAAACw1apUlpM6d+6ct3316tUxf/78+OCDDyJJkoiIqFKlStmrAwAAAAAAKIfKFMCMHj06MplM3mNrg5dMJhOZTKbEsAYAAAAAAOD7KvUlyNYGM0mSRL169eKPf/xj2pcAAAAAAADYqpVpBkzTpk1LnAFTuXLlaNiwYXTs2DF+/etfR/369TepQAAAAAAAgPKmTAHMzJkzUy4DAAAAAADg+yP1JcgAAAAAAAC2dWWaARMR8e6778bEiRPjyy+/jIiI+vXrx3777Rd77713asUBAAAAAACUR6UOYO6888647rrrYvbs2XmPN23aNH7729/GmWeeucnFAQAAAAAAlEcFL0G2cuXKOOGEE+Kcc86JWbNmRZIkeR+zZs2Ks88+O0444YRYtWrV5qwdAAAAAABgq1RwAHPOOefEM888E0mSRCaTiUwmk9NnbXuSJDFixIg455xzUi0WAAAAAACgPChoCbJJkybFPffcUxy6JEkSu+++exx88MGx4447RkTE//73v3j99ddj6tSpxSHMPffcE2effXa0adNmsz0BAAAAAACArU1BAcw999xT/N+NGjWK+++/Pzp37py376hRo6J3797x2WefRUTE3/72t7jttttSKBUAAAAAAKB8KGgJsjFjxqzpXKFCPP/88yWGLxERnTp1iueeey4qVFgz9L///e8UygQAAAAAACg/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gAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGUCGAAAAAAAgJQJYAAAAAAAAFImgAEAAAAAAEiZAAYAAAAAACBlAhgAAAAAAICUCWAAAAAAAABSJoABAAAAAABImQAGAAAAAAAgZQIYAAAAAACAlAlgAAAAAAAAUiaAAQAAAAAASJkABgAAAAAAIGVVPoAZM2ZMvksAAAAAAAA4LlU+gClLkiQxZcqUaN68edSuXTsuvfTSWLt2bc46mUwm5s+fn23v378/rr322mjZsmX84Q9/qOSKAQAAAACAk0mVDGD+8pe/xOjRo6N169bxL//yL3HWWWfF17/+9di3b19ERMycOTMefvjhePTRR2PVqlVRt27duOyyy+Ljjz8uc3979+6NoUOHxiuvvBLLly+Pzp07V+bpAAAAAAAAJ5kqGcBMmDAhVq5cGXPnzo3BgwfHY489FmeccUaUlpZGkiTx4IMPxuTJk+PKK6+Mrl27xhNPPBGbNm3KmfFy0I4dO2LAgAGxadOmWL58ebRr167yTwgAAAAAADipVM93AWV57bXXYtSoUVFcXByzZ8+Oiy++OC6++OKIiNiwYUNs2bIlLr300uz6DRo0iK997Wvx8ssvx4gRI7LLt2zZEsXFxVGvXr1YsmRJnHrqqRUet6SkJEpKSrLtDz/8MCIidu7cmeLZ5deBko/yXQIAJ4kv0/8/v2x2fXwg3yUAcBIxJqi6Pvnok3yXAMBJ4ss0Hjh4LkmSHHXdKhnA9O7dO2bPnh3nnnvuEX1btmyJiIiioqKc5UVFRdm+g8aPHx9nnHFGLFy4MOrUqXPU4953330xbdq0I5a3atXqeMoHACKiwY/G5rsEAKAquK9BvisAAPKswe1fvvHArl27okGDis+rSgYwP/jBD+Lee++NCRMmxPr16+P111+PsWPHxtixx/dDzpAhQ2L+/Pnxk5/8JCZMmHDU9b/3ve/Fbbfdlm2XlpbGBx98EKeddlpkMpnjPg/gi2/nzp3RqlWrePfdd6N+/fr5LgcAyBNjAgAgwpgA+HTmy65du6JFixZHXbdKBjB169aNGTNmxIwZM2LYsGExaNCgmDBhQhQUFGRvPbZ169Zo3rx5dputW7dGt27dcvYzcuTIGDp0aNxwww2RJElOuFKWwsLCKCwszFl2tNuWASeH+vXrG1gBAMYEAEBEGBPAye5oM18OKvic6zhhp556atx8880xaNCgWLZsWbRr1y6aNWsWv/nNb7Lr7Ny5M1atWhUXXHDBEduPHj065syZE5MmTYrvf//7lVk6AAAAAABwkqqSM2AmTJgQw4YNi27dusWBAwdi0aJFsWTJkpg8eXJkMpn49re/Hffcc0+0b98+2rVrF3feeWe0aNEihg0bVub+Ro4cGQUFBTF69OhIkiS++93vVu4JAQAAAAAAJ5UqGcC0bt06brvttli7dm3s2bMnFi9eHDfccEPceuutERExadKk2LNnT9x0002xY8eO6NOnT7zwwgtRq1atcvd53XXXRUFBQYwcOTJKS0vj9ttvr6zTAb7ACgsLY+rUqUfcnhAAOLkYEwAAEcYEwPHJJEmS5LuIiowZMybmzJmT7zIAAAAAAACOWZV/BgwAAAAAAMAXTZWfAQMAAAAAAPBFYwYMAAAAAABAygQwAAAAAAAAKRPAAAAAHObAgQM57VWrVsXSpUtj//79eaoIAAD4ohHAAETE/v37Y9KkSXHWWWdFz549Y9asWTn9W7dujWrVquWpOgCgsmzevDn69OkThYWFUVxcHNu3b48hQ4bEBRdcEP369YvOnTvH5s2b810mAJBne/bsiaVLl+a7DKCKE8AARMSMGTPiiSeeiLFjx8bAgQPjtttui5tvvjlnnSRJ8lQdAFBZbr/99kiSJJ555plo3rx5DBkyJHbu3BnvvvtubNy4MZo0aRIzZszId5kAQJ6tW7cuLr744nyXAVRxmcQvigDRvn37+OEPfxhDhgyJiE8HUoMGDYo+ffrErFmz4v33348WLVoccTsSAODLpUWLFvH0009Hr1694oMPPojGjRvHwoUL45JLLomIiBdffDFuvPHGWL9+fZ4rBQDy6Y033ojzzjvP7wRAharnuwCAquC9996Lzp07Z9tnnXVWLF68OPr37x8jR46MmTNn5rE6AKCybN++PVq2bBkREY0aNYo6depEmzZtsv1nnXWWW5ABwEmgUaNGFfYLXoBjIYABiIhmzZrF+vXro23bttllLVu2jEWLFsXFF18cY8aMyVttAEDladq0aWzevDlatWoVERHjxo3L+QFm+/btUbdu3XyVBwBUkpKSkrjllluiS5cuZfa/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VVJSEo8++mhccskl0aRJk6hZs2acdtpp0bdv33jwwQdj79695W77+9//Pm655Zbo1KlTnHLKKVG7du1o06ZNDBs2LObPn59dL0mSmDlzZlxzzTVxzjnnRFFRUdSsWTPq1asXHTt2jL//+7+PN954I2ffY8aMiUwmE0uWLMkue+edd8q8XduxPANm586dMXPmzOjdu3c0atQoatSoEU2bNo0BAwbErFmzcgLAw2s4+EqSJObMmRM9e/aMOnXqRMOGDeOqq66KtWvXHt+Hfox2794dDRo0yB5/1KhRR6yzffv2qFmzZnadW2+9NSLK/kw2bNgQ3/jGN6KoqChq1aoV3bp1i5///OflHn/btm0xffr0+NrXvhYNGzaMmjVrRsuWLeOaa66JFStWfC7nDAAAERGRAAAAJ2zo0KFJRGRfTz75ZLnrXn755Tnrzp8/P9tXXFycXd6mTZsjtm3Tpk22v7i4OEmSJFm0aFHO/sp7HcsxPvnkk+T666+vcD/t27dPNmzYUGZN5b1mz559TJ/joduUdf5lee+995Jzzz23wuN36NAhp+aD7rnnnqSgoKDc7a688srsuvv37z/qedaoUSN56qmnstuMHj36qNuMHj06SZIkefvtt3OWT506NafWN998M2nbtm2F++rVq1eybdu2nO0Or2HEiBFlbltUVJS8//77x/SZl6Wiv9348eOzfbVr1062b9+e0//zn/88Z/vVq1eX+ZkMHTo0adCgQZn1jx8//oiaXnrppaSoqKjczyuTySR33333Zz5nAACoiBkwAACQgtWrV+e0L7jggnLXPbzv8G2PV5MmTWL48OFx/vnn5yzv27dvDB8+PPs6FlOmTInZs2dn282aNYtBgwZFr169srNc1q5dG1deeWV88sknERExePDgI/bfpk2bnGN/Xg+eT5Ik/vZv/zZn5kmzZs1i4MCBcfrpp2eXvfXWWzF06NBszRERs2bNismTJ0dpaWl2Wdu2bWPQoEHRu3fvKCwsLPOYTZo0iZ49e8agQYPiiiuuiO7du0e1atUiImL//v0xduzY2LNnT0RE9OjRI4YPHx6NGzfObl+nTp2cz6ZHjx5HPc+9e/fG4MGDY+PGjdllbdq0iYEDB0aTJk2yy1auXBnXXXddhft68skno6ioKC699NJo2LBhdvnWrVvjkUceOWotn8W4ceOy35+PPvoo5s6dm9P/1FNPZd9369YtvvrVr5a5n3//93+PvXv3xkUXXXTE9/2hhx6KF154IdvesmVLXHHFFbF169aI+PQWab169YrBgwdHUVFRRHz6/ZkyZUrO8QEAIDX5ToAAAODLoFatWjlX1peUlJS77qOPPpqz7i233JLt+ywzYA6aPXt2zn4XLVpU5vHLO8af//znnPMYOnRosm/fvmz/U089lbP/X/ziFzn7PbTv4KyO43XoPo5lBsyzzz6bs02fPn2S3bt3J0mSJB999FEyYMCAMmv+5JNPkmbNmuX0PfTQQ0lpaWl239u2bUueeeaZbLu0tDR54403ctY56Pnnn8/Z13PPPZfTf7S/a5JUPAPmoYceyun7+te/nuzfvz9JkiT54IMPkq5du+b0r1ixIrvt4TNgLrzwwmTXrl1JkiTJ+vXrk8LCwnK/U8fjaH+7wYMHZ/u7dOmSXb5t27akevXq2b6HH3643M+koKAg53v94x//OKd/4MCB2b7vfOc72eXVqlXL+Uz27t2bnH/++dn+s88++zOfNwAAlMcMGAAAICIiXnzxxfj444+z7ffffz+uvfbauPrqq+Pqq6+OX/7ylznrP//885Vd4hEOr2HKlClRt27diIioVatWTJs2Laf/4AyJV199NbZs2ZJdPmDAgPjWt76V8yybRo0axbBhw7LtTCYTDRs2jEmTJkX37t2jYcOGUb169chkMjFo0KCc46xZsyaV8zvo8POcMWNG9llCDRs2jDvuuCOn/9CZIIe7++67o169ehERccYZZ8TZZ5+d7du8eXNaJR/h4HNdIj597s7KlSsjIuLpp5/OzkwqLCyscAbPgAEDss8+ioi4+eabo1WrVtn2smXL4sCBAxERsWDBguzyunXrxg9+8IPsd3nkyJGxc+fObP+aNWti/fr1J3aCAABwmOpHXwUAADiaxo0bx5/+9Kdse8uWLdG6desy1z14S6SDDr2FVD4denuriMj+QF6ed95553Os5tgcXsM555xTYfvg+oefa+/evY96rNdffz369esXH3744VHXPfTH/TQcep6FhYVx1lln5fSXd55lOfz2Xg0aNMi+LykpOZEyK3TZZZfF2WefnQ2nHnvssejVq1fO7b+GDh0ajRo1KncfnTp1ymkXFBREx44d4913342IT29v9pe//CWKiopy/sY7d+6MefPmVVjfO++8E2eeeebxnhYAAJTLDBgAAEjBeeedl9OuKLx4+eWXK9z2oINX8h/q/fff/wzVfT727t2b7xIq1R133JETvjRv3jwGDRoUw4cPP2IGTJIklV3eMTs84Dj4/JrPWyaTiXHjxmXb//qv/xobNmyIRYsWZZfdcMMNlVJLWU627zMAAJ8/AQwAAKTgiiuuyGmX9zDzNWvWxK9//etsu27dujm3VKpZs2b2/Y4dO3K2ff311+Ojjz4qt4ZDb5/1WbRp0yan/fjjj0eSJOW+/uu//uuEjpeGw2cZ/fGPf8xpv/nmm2Wu37Zt25zlK1asOOqxXnrppez7r371q7Fx48ZYsGBB/OpXv4opU6ZUuO2J/m0OPc+SkpIjbpdV3nlWNWPGjIlTTjklIiL27NkT1157bfb2Y6effnoMHDiwwu0P//smSRJvvfVWtl27du047bTTIiL3+3zGGWdU+F1OkiSGDBmSyjkCAMBBAhgAAEjByJEj4/TTT8+2ly5dGt/5zneyPy5HfHqLo6uvvjpn2T/8wz/k3AKqWbNm2fe7d++OJ598MiIitm/fnjN7oCy1a9fOaW/atOm4zqF///45AdD06dOPuJVVkiSxcuXKGDt2bKxatarc4x/vsT+rw2eeTJ8+PTuToaSk5IhnwFx++eUREdG9e/ecz3rhwoXx8MMP58xc2bFjRzz77LPZ9v79+7PvCwsLo0aNGhHx6cyJqVOnVljnoZ/Ntm3bYt++fcd0fgcdfp6TJ0/Ofo927NgRM2fOzOk/eJ5VzSmnnBJjxozJtn/7299m348ePToKCir+J+rChQtjyZIl2fZPf/rT+N///d9su0+fPtln4xz6mW3YsCHuv//+KC0tzdnftm3b4rHHHovx48d/pvMBAICKZJKqPDceAAC+QJYvXx79+/fP+aG+WbNm0b1799i9e3e89NJLOX3nnXderFixImrVqpVdNmvWrPjmN7+Zs99WrVrFli1bcraNiCguLo7Fixdn26tXr47u3btn2w0aNIg+ffpErVq1omfPnjFp0qSIiOjXr1/2R+w2bdrkPCtj0qRJ8cADD2Tb1atXjx49ekSTJk1i+/bt8Yc//CG2b98eERGLFi3Kmb3TtWvX+P3vf59t9+7dOxtyPPHEE1GnTp2KP8DInSlSp06dI4KHg9q1axcPPPBAlJaWRs+ePePVV1/N9jVv3jy6du0af/zjH7PPBon49Pkhr7/+ejY4+dnPfhY33nhjzn7btm0bnTp1it27d8crr7wSAwcOjPnz50dERN++fWPZsmXZdTt06BBnnnlmrF69OrZu3ZoT3kydOjXuuuuubPtb3/pW/OhHP8q2v/KVr0THjh2joKAgbr/99ujRo0ds3Lgx2rVrV+Y+du/eHZ06dco5n7Zt20aHDh3itddey7k13YABA+I///M/s+0xY8bE448/nm0f/k/Air4Px+PQv11F+1mzZk107NjxiDrWrl17xLNtDv9MIiJq1KgRvXr1io8//jheeeWVnL4FCxZkvzObNm2Kc845J2cmWevWraNTp05RUFAQb7/9drz11ltRWlp6xH9LAACQigQAAEjNkiVLkubNmycRUeFryJAhyfbt24/Yfu/evUmHDh3K3aZFixbZdnFxcc62paWlSefOncvc9sorr8yuV1xcnF3epk2bnH188sknyahRo45af0QkS5cuzdn2gQceKHfdss61LMdy3IhIzj333Ow27777btKlS5cK12/fvn2ybt26I4531113JQUFBeVud+jntmTJkqR69epHrJPJZJK77747Z9nUqVNzjrNq1apyj/PMM88kSZIkb7/9doX7+N3vfpe0bt26wvPs0aNH8uc//zlnu9GjR+esc7iKvg/H49BjHG0/l19+ec76ffv2LXO9wz+TESNGJE2aNCnz3MeNG3fE9suWLUuaNm161O9T//79P/N5AwBAedyCDAAAUtS3b99Yt25d/PM//3Ncfvnl0bx586hZs2bUq1cv2rdvH9dff328+OKL8R//8R9x6qmnHrF97dq1Y9GiRTFq1Kho3LhxFBYWxjnnnBMPPvhgzJ8/Pzt7oyyZTCYWLFgQI0aMiKZNmx71dk5lqVatWjz++OPxm9/8Jv7u7/4u2rVrF7Vr144aNWpEs2bNori4OCZPnhyvvfZaXHTRRTnb3nbbbXH//fdHx44dc25l9nk7/fTT47e//W088sgj0a9fv2jUqFFUr149GjZsGL17945/+qd/itWrV8eZZ555xLZTp06NV199NW666abo2LFj1K1bNwoLC6NVq1YxdOjQnNtl9e3bNzvrp06dOlGvXr246KKLYsGCBTFy5MgKa+zZs2fMmzcvevXqFXXr1v1M59mlS5f43e9+F/fdd1/06tUrTj311KhevXqcdtpp0b9//3jsscdixYoV0bhx48+0/8p066235rRvuOGGY9quQ4cO8eqrr8aoUaOiadOmUVhYGF26dImf/vSn8fDDDx+xfp8+feK///u/Y8aMGXHhhRdGw4YNo1q1alGvXr34yle+Etddd1088cQTObeaAwCAtLgFGQAAAJVq7ty5MWrUqIiIqF+/fmzatKnMYKqi27IBAEBVVz3fBQAAAPDl9+abb8bzzz8fW7ZsiZ/97GfZ5TfddNNnnhUEAABVmQAGAACAz90rr7wS3/3ud3OWtWvXLv7xH/8xTxUBAMDnyzNgAAAAqDSZTCaaN28eo0ePjmXLlpX5LCQAAPgy8AwYAAAAAACAlJkBAwAAAAAAkDIBDAAAAAAAQMoEMAAAAAAAACkTwAAAAAAAAKRMAAMAAAAAAJAyAQwAAAAAAEDKBDAAAAAAAAApE8AAAAAAAACkTAADAAAAAACQsv8Dl3t/yqFfp5oAAAAASUVORK5CYII=\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "##The top and bottom earning locations for item sales are as follows.\n",
        "The Top earning location for Items sales\n",
        "- Tier 2\n",
        "\n",
        "The bottom earning location for item sales.\n",
        "- Tier 1"
      ],
      "metadata": {
        "id": "YGS7nXSSCcsF"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Machine Learning (Part 5)"
      ],
      "metadata": {
        "id": "q7Rm73wRm__s"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "import pandas as pd \n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "import seaborn as sns\n",
        "\n",
        "## Modeling & preprocessing import\n",
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.preprocessing import OneHotEncoder,StandardScaler\n",
        "from sklearn.compose import ColumnTransformer,make_column_transformer,make_column_selector\n",
        "from sklearn.pipeline import Pipeline, make_pipeline\n",
        "from sklearn.impute import SimpleImputer      "
      ],
      "metadata": {
        "id": "h3o02NiFoWGW"
      },
      "execution_count": 40,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#Reloading in the Data.\n",
        "df2.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 351
        },
        "id": "svQDOJovotPF",
        "outputId": "80bd7383-d615-417b-b1b2-a08f67873024"
      },
      "execution_count": 41,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "  Item_Identifier  Item_Weight Item_Fat_Content  Item_Visibility  \\\n",
              "0           FDA15         9.30          Low Fat         0.016047   \n",
              "1           DRC01         5.92          Regular         0.019278   \n",
              "2           FDN15        17.50          Low Fat         0.016760   \n",
              "3           FDX07        19.20          Regular         0.000000   \n",
              "4           NCD19         8.93          Low Fat         0.000000   \n",
              "\n",
              "               Item_Type  Item_MRP Outlet_Identifier  \\\n",
              "0                  Dairy  249.8092            OUT049   \n",
              "1            Soft Drinks   48.2692            OUT018   \n",
              "2                   Meat  141.6180            OUT049   \n",
              "3  Fruits and Vegetables  182.0950            OUT010   \n",
              "4              Household   53.8614            OUT013   \n",
              "\n",
              "   Outlet_Establishment_Year Outlet_Size Outlet_Location_Type  \\\n",
              "0                       1999      Medium               Tier 1   \n",
              "1                       2009      Medium               Tier 3   \n",
              "2                       1999      Medium               Tier 1   \n",
              "3                       1998         NaN               Tier 3   \n",
              "4                       1987        High               Tier 3   \n",
              "\n",
              "         Outlet_Type  Item_Outlet_Sales  \n",
              "0  Supermarket Type1          3735.1380  \n",
              "1  Supermarket Type2           443.4228  \n",
              "2  Supermarket Type1          2097.2700  \n",
              "3      Grocery Store           732.3800  \n",
              "4  Supermarket Type1           994.7052  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-1ca0cadc-9cbd-490b-b98e-c1eed1f0e433\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Identifier</th>\n",
              "      <th>Item_Weight</th>\n",
              "      <th>Item_Fat_Content</th>\n",
              "      <th>Item_Visibility</th>\n",
              "      <th>Item_Type</th>\n",
              "      <th>Item_MRP</th>\n",
              "      <th>Outlet_Identifier</th>\n",
              "      <th>Outlet_Establishment_Year</th>\n",
              "      <th>Outlet_Size</th>\n",
              "      <th>Outlet_Location_Type</th>\n",
              "      <th>Outlet_Type</th>\n",
              "      <th>Item_Outlet_Sales</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>FDA15</td>\n",
              "      <td>9.30</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.016047</td>\n",
              "      <td>Dairy</td>\n",
              "      <td>249.8092</td>\n",
              "      <td>OUT049</td>\n",
              "      <td>1999</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 1</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "      <td>3735.1380</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>DRC01</td>\n",
              "      <td>5.92</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.019278</td>\n",
              "      <td>Soft Drinks</td>\n",
              "      <td>48.2692</td>\n",
              "      <td>OUT018</td>\n",
              "      <td>2009</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type2</td>\n",
              "      <td>443.4228</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>FDN15</td>\n",
              "      <td>17.50</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.016760</td>\n",
              "      <td>Meat</td>\n",
              "      <td>141.6180</td>\n",
              "      <td>OUT049</td>\n",
              "      <td>1999</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 1</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "      <td>2097.2700</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>FDX07</td>\n",
              "      <td>19.20</td>\n",
              "      <td>Regular</td>\n",
              "      <td>0.000000</td>\n",
              "      <td>Fruits and Vegetables</td>\n",
              "      <td>182.0950</td>\n",
              "      <td>OUT010</td>\n",
              "      <td>1998</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Grocery Store</td>\n",
              "      <td>732.3800</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>NCD19</td>\n",
              "      <td>8.93</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>0.000000</td>\n",
              "      <td>Household</td>\n",
              "      <td>53.8614</td>\n",
              "      <td>OUT013</td>\n",
              "      <td>1987</td>\n",
              "      <td>High</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "      <td>994.7052</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-1ca0cadc-9cbd-490b-b98e-c1eed1f0e433')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-1ca0cadc-9cbd-490b-b98e-c1eed1f0e433 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-1ca0cadc-9cbd-490b-b98e-c1eed1f0e433');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 41
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# Checking for Duplicates\n",
        "df2.duplicated().sum()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "iM8SJ1tDq_Lx",
        "outputId": "73036035-3a04-4eec-e15e-ec7bcc513b61"
      },
      "execution_count": 42,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0"
            ]
          },
          "metadata": {},
          "execution_count": 42
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# Checking missing values amd info\n",
        "print(df2.info(), '\\n')\n",
        "print(df2.isna().sum())"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "p5f9B6XDrJZV",
        "outputId": "c5d797a2-94bf-4c38-98cb-65020f8f488f"
      },
      "execution_count": 43,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 8523 entries, 0 to 8522\n",
            "Data columns (total 12 columns):\n",
            " #   Column                     Non-Null Count  Dtype  \n",
            "---  ------                     --------------  -----  \n",
            " 0   Item_Identifier            8523 non-null   object \n",
            " 1   Item_Weight                7060 non-null   float64\n",
            " 2   Item_Fat_Content           8523 non-null   object \n",
            " 3   Item_Visibility            8523 non-null   float64\n",
            " 4   Item_Type                  8523 non-null   object \n",
            " 5   Item_MRP                   8523 non-null   float64\n",
            " 6   Outlet_Identifier          8523 non-null   object \n",
            " 7   Outlet_Establishment_Year  8523 non-null   int64  \n",
            " 8   Outlet_Size                6113 non-null   object \n",
            " 9   Outlet_Location_Type       8523 non-null   object \n",
            " 10  Outlet_Type                8523 non-null   object \n",
            " 11  Item_Outlet_Sales          8523 non-null   float64\n",
            "dtypes: float64(4), int64(1), object(7)\n",
            "memory usage: 799.2+ KB\n",
            "None \n",
            "\n",
            "Item_Identifier                 0\n",
            "Item_Weight                  1463\n",
            "Item_Fat_Content                0\n",
            "Item_Visibility                 0\n",
            "Item_Type                       0\n",
            "Item_MRP                        0\n",
            "Outlet_Identifier               0\n",
            "Outlet_Establishment_Year       0\n",
            "Outlet_Size                  2410\n",
            "Outlet_Location_Type            0\n",
            "Outlet_Type                     0\n",
            "Item_Outlet_Sales               0\n",
            "dtype: int64\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "for column in df2.select_dtypes(include = 'object'):\n",
        "  print(f\"{column} value counts: \\n{df2[column].value_counts()}\\n\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "FYiniL7qrjmX",
        "outputId": "75737ce0-98e6-4aae-9334-15eb513097d2"
      },
      "execution_count": 44,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Item_Identifier value counts: \n",
            "FDW13    10\n",
            "FDG33    10\n",
            "NCY18     9\n",
            "FDD38     9\n",
            "DRE49     9\n",
            "         ..\n",
            "FDY43     1\n",
            "FDQ60     1\n",
            "FDO33     1\n",
            "DRF48     1\n",
            "FDC23     1\n",
            "Name: Item_Identifier, Length: 1559, dtype: int64\n",
            "\n",
            "Item_Fat_Content value counts: \n",
            "Low Fat    5089\n",
            "Regular    2889\n",
            "LF          316\n",
            "reg         117\n",
            "low fat     112\n",
            "Name: Item_Fat_Content, dtype: int64\n",
            "\n",
            "Item_Type value counts: \n",
            "Fruits and Vegetables    1232\n",
            "Snack Foods              1200\n",
            "Household                 910\n",
            "Frozen Foods              856\n",
            "Dairy                     682\n",
            "Canned                    649\n",
            "Baking Goods              648\n",
            "Health and Hygiene        520\n",
            "Soft Drinks               445\n",
            "Meat                      425\n",
            "Breads                    251\n",
            "Hard Drinks               214\n",
            "Others                    169\n",
            "Starchy Foods             148\n",
            "Breakfast                 110\n",
            "Seafood                    64\n",
            "Name: Item_Type, dtype: int64\n",
            "\n",
            "Outlet_Identifier value counts: \n",
            "OUT027    935\n",
            "OUT013    932\n",
            "OUT049    930\n",
            "OUT046    930\n",
            "OUT035    930\n",
            "OUT045    929\n",
            "OUT018    928\n",
            "OUT017    926\n",
            "OUT010    555\n",
            "OUT019    528\n",
            "Name: Outlet_Identifier, dtype: int64\n",
            "\n",
            "Outlet_Size value counts: \n",
            "Medium    2793\n",
            "Small     2388\n",
            "High       932\n",
            "Name: Outlet_Size, dtype: int64\n",
            "\n",
            "Outlet_Location_Type value counts: \n",
            "Tier 3    3350\n",
            "Tier 2    2785\n",
            "Tier 1    2388\n",
            "Name: Outlet_Location_Type, dtype: int64\n",
            "\n",
            "Outlet_Type value counts: \n",
            "Supermarket Type1    5577\n",
            "Grocery Store        1083\n",
            "Supermarket Type3     935\n",
            "Supermarket Type2     928\n",
            "Name: Outlet_Type, dtype: int64\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# fix Item_Fat_Content inconsistencies\n",
        "df2.replace({'LF': 'Low Fat', \n",
        "            'reg': 'Regular', \n",
        "            'low fat': 'Low Fat'},\n",
        "           inplace = True)"
      ],
      "metadata": {
        "id": "Ww3_gMl9sBAB"
      },
      "execution_count": 45,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# check Item_Fat_Content to see fix\n",
        "df2['Item_Fat_Content'].value_counts()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "P_-dq3BtsCRw",
        "outputId": "80ed7204-410f-48bc-d141-9e95cbb6a8d6"
      },
      "execution_count": 46,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Low Fat    5517\n",
              "Regular    3006\n",
              "Name: Item_Fat_Content, dtype: int64"
            ]
          },
          "metadata": {},
          "execution_count": 46
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Explore Categorical, Numerical or Ordinal data"
      ],
      "metadata": {
        "id": "zAqY-Op2sVKC"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# explore object data\n",
        "df2.describe(include = 'object')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 172
        },
        "id": "s-ZdTS5ksboh",
        "outputId": "1cf19e1d-e640-46f1-e845-3dd0172289e3"
      },
      "execution_count": 47,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "       Item_Identifier Item_Fat_Content              Item_Type  \\\n",
              "count             8523             8523                   8523   \n",
              "unique            1559                2                     16   \n",
              "top              FDW13          Low Fat  Fruits and Vegetables   \n",
              "freq                10             5517                   1232   \n",
              "\n",
              "       Outlet_Identifier Outlet_Size Outlet_Location_Type        Outlet_Type  \n",
              "count               8523        6113                 8523               8523  \n",
              "unique                10           3                    3                  4  \n",
              "top               OUT027      Medium               Tier 3  Supermarket Type1  \n",
              "freq                 935        2793                 3350               5577  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-accddfaf-aa7a-4ef2-8e65-3c51737bdd18\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Identifier</th>\n",
              "      <th>Item_Fat_Content</th>\n",
              "      <th>Item_Type</th>\n",
              "      <th>Outlet_Identifier</th>\n",
              "      <th>Outlet_Size</th>\n",
              "      <th>Outlet_Location_Type</th>\n",
              "      <th>Outlet_Type</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "      <td>6113</td>\n",
              "      <td>8523</td>\n",
              "      <td>8523</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>unique</th>\n",
              "      <td>1559</td>\n",
              "      <td>2</td>\n",
              "      <td>16</td>\n",
              "      <td>10</td>\n",
              "      <td>3</td>\n",
              "      <td>3</td>\n",
              "      <td>4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>top</th>\n",
              "      <td>FDW13</td>\n",
              "      <td>Low Fat</td>\n",
              "      <td>Fruits and Vegetables</td>\n",
              "      <td>OUT027</td>\n",
              "      <td>Medium</td>\n",
              "      <td>Tier 3</td>\n",
              "      <td>Supermarket Type1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>freq</th>\n",
              "      <td>10</td>\n",
              "      <td>5517</td>\n",
              "      <td>1232</td>\n",
              "      <td>935</td>\n",
              "      <td>2793</td>\n",
              "      <td>3350</td>\n",
              "      <td>5577</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-accddfaf-aa7a-4ef2-8e65-3c51737bdd18')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-accddfaf-aa7a-4ef2-8e65-3c51737bdd18 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-accddfaf-aa7a-4ef2-8e65-3c51737bdd18');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 47
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# explore numeric data\n",
        "df2.describe(include = 'number')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 295
        },
        "id": "rwrnS3ljsdPa",
        "outputId": "ec2b6cdf-1435-4816-bc36-47cbd0a6d376"
      },
      "execution_count": 48,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "       Item_Weight  Item_Visibility     Item_MRP  Outlet_Establishment_Year  \\\n",
              "count  7060.000000      8523.000000  8523.000000                8523.000000   \n",
              "mean     12.857645         0.066132   140.992782                1997.831867   \n",
              "std       4.643456         0.051598    62.275067                   8.371760   \n",
              "min       4.555000         0.000000    31.290000                1985.000000   \n",
              "25%       8.773750         0.026989    93.826500                1987.000000   \n",
              "50%      12.600000         0.053931   143.012800                1999.000000   \n",
              "75%      16.850000         0.094585   185.643700                2004.000000   \n",
              "max      21.350000         0.328391   266.888400                2009.000000   \n",
              "\n",
              "       Item_Outlet_Sales  \n",
              "count        8523.000000  \n",
              "mean         2181.288914  \n",
              "std          1706.499616  \n",
              "min            33.290000  \n",
              "25%           834.247400  \n",
              "50%          1794.331000  \n",
              "75%          3101.296400  \n",
              "max         13086.964800  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-a53a6997-f23c-4749-b35e-c5a65985b9ab\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Item_Weight</th>\n",
              "      <th>Item_Visibility</th>\n",
              "      <th>Item_MRP</th>\n",
              "      <th>Outlet_Establishment_Year</th>\n",
              "      <th>Item_Outlet_Sales</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>7060.000000</td>\n",
              "      <td>8523.000000</td>\n",
              "      <td>8523.000000</td>\n",
              "      <td>8523.000000</td>\n",
              "      <td>8523.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>12.857645</td>\n",
              "      <td>0.066132</td>\n",
              "      <td>140.992782</td>\n",
              "      <td>1997.831867</td>\n",
              "      <td>2181.288914</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>4.643456</td>\n",
              "      <td>0.051598</td>\n",
              "      <td>62.275067</td>\n",
              "      <td>8.371760</td>\n",
              "      <td>1706.499616</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>4.555000</td>\n",
              "      <td>0.000000</td>\n",
              "      <td>31.290000</td>\n",
              "      <td>1985.000000</td>\n",
              "      <td>33.290000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>8.773750</td>\n",
              "      <td>0.026989</td>\n",
              "      <td>93.826500</td>\n",
              "      <td>1987.000000</td>\n",
              "      <td>834.247400</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>12.600000</td>\n",
              "      <td>0.053931</td>\n",
              "      <td>143.012800</td>\n",
              "      <td>1999.000000</td>\n",
              "      <td>1794.331000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>16.850000</td>\n",
              "      <td>0.094585</td>\n",
              "      <td>185.643700</td>\n",
              "      <td>2004.000000</td>\n",
              "      <td>3101.296400</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>21.350000</td>\n",
              "      <td>0.328391</td>\n",
              "      <td>266.888400</td>\n",
              "      <td>2009.000000</td>\n",
              "      <td>13086.964800</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-a53a6997-f23c-4749-b35e-c5a65985b9ab')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-a53a6997-f23c-4749-b35e-c5a65985b9ab button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-a53a6997-f23c-4749-b35e-c5a65985b9ab');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 48
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Categorical Columns:\n",
        "( Item_Identifier\", \"Item_Fat_Content\", \"Item_Type\", \"Outlet_Identifier\", \"Outlet_Location_Type\", \"Outlet_Type\")\n",
        "\n",
        "# Numerical Columns \n",
        "(\"Item_Weight\", \"Item_Visibility\", \"Item_MRP\")\n",
        "\n",
        "# Ordinal Columns\n",
        "(\"Outlet_size\")\n"
      ],
      "metadata": {
        "id": "27192NYtswkd"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df2['Item_Identifier'].unique()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "BZiZRoZ93Vn-",
        "outputId": "07628b73-8f56-432c-935a-c620bc2c2235"
      },
      "execution_count": 49,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "array(['FDA15', 'DRC01', 'FDN15', ..., 'NCF55', 'NCW30', 'NCW05'],\n",
              "      dtype=object)"
            ]
          },
          "metadata": {},
          "execution_count": 49
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# Item_Identifier contains 1559 uniques values \n",
        "#These values will not predict how much will be sold. Therefore this column will be dropped\n",
        "df2.drop(columns = 'Item_Identifier', inplace = True)"
      ],
      "metadata": {
        "id": "dbD-h6sdu1po"
      },
      "execution_count": 50,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Display the number of duplicate rows in the dataset\n",
        "print(f'There are {df2.duplicated().sum()} duplicate rows.')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "nrmOsJYE2Zpb",
        "outputId": "3bfc546f-ffd4-4eee-8db3-b8d876bddcfd"
      },
      "execution_count": 51,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "There are 0 duplicate rows.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "#Divide features and target and perform a train/test split"
      ],
      "metadata": {
        "id": "y2u972hBv6Lq"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# features (X) = \n",
        "X= df2[['Item_Weight', 'Item_Fat_Content', 'Item_Visibility', 'Item_Type', 'Item_MRP', 'Outlet_Identifier', 'Outlet_Establishment_Year', 'Outlet_Size', 'Outlet_Location_Type', 'Outlet_Type']] \n",
        "# target (y) =  \"Item_Outlet_Sales\"\n",
        "target = 'Item_Outlet_Sales'\n",
        "y = df2[target]\n"
      ],
      "metadata": {
        "id": "zSgra_8Ur1-f"
      },
      "execution_count": 52,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Perfoming a train-test-split\n",
        "X_train, X_test, y_train, y_test = train_test_split(X, y, random_state = 42)"
      ],
      "metadata": {
        "id": "aZ7GUdbyymw6"
      },
      "execution_count": 53,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "y.dtype"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "vvAEerjILQvn",
        "outputId": "80bf2372-2ef5-41fd-8478-ebca507b4a53"
      },
      "execution_count": 54,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "dtype('float64')"
            ]
          },
          "metadata": {},
          "execution_count": 54
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "y.value_counts(dropna = False)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "cJVy33aWLYyO",
        "outputId": "8f1e005b-891d-48d3-d9e6-9c5bd9e1a73b"
      },
      "execution_count": 55,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "958.7520     17\n",
              "1342.2528    16\n",
              "703.0848     15\n",
              "1845.5976    15\n",
              "1278.3360    14\n",
              "             ..\n",
              "4124.6310     1\n",
              "6622.7126     1\n",
              "1614.5650     1\n",
              "5602.7070     1\n",
              "2778.3834     1\n",
              "Name: Item_Outlet_Sales, Length: 3493, dtype: int64"
            ]
          },
          "metadata": {},
          "execution_count": 55
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# inspect value counts for each column in X_train\n",
        "for column in X_train:\n",
        "  print(f\"{column}: {df2[column].dtype}\\n{X_train[column].value_counts(dropna = False)} \\n\\n\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "IvxPgWq1LtKZ",
        "outputId": "8686001d-5b59-4de6-f7d1-f9c930a51c29"
      },
      "execution_count": 56,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Item_Weight: float64\n",
            "NaN       1107\n",
            "12.150      62\n",
            "17.600      60\n",
            "13.650      57\n",
            "11.800      55\n",
            "          ... \n",
            "4.590        1\n",
            "8.485        1\n",
            "6.170        1\n",
            "7.685        1\n",
            "5.155        1\n",
            "Name: Item_Weight, Length: 413, dtype: int64 \n",
            "\n",
            "\n",
            "Item_Fat_Content: object\n",
            "Low Fat    4129\n",
            "Regular    2263\n",
            "Name: Item_Fat_Content, dtype: int64 \n",
            "\n",
            "\n",
            "Item_Visibility: float64\n",
            "0.000000    400\n",
            "0.076975      2\n",
            "0.059847      2\n",
            "0.014048      2\n",
            "0.061164      2\n",
            "           ... \n",
            "0.062276      1\n",
            "0.021337      1\n",
            "0.108005      1\n",
            "0.180821      1\n",
            "0.016993      1\n",
            "Name: Item_Visibility, Length: 5927, dtype: int64 \n",
            "\n",
            "\n",
            "Item_Type: object\n",
            "Fruits and Vegetables    948\n",
            "Snack Foods              906\n",
            "Household                695\n",
            "Frozen Foods             632\n",
            "Dairy                    507\n",
            "Canned                   481\n",
            "Baking Goods             478\n",
            "Health and Hygiene       390\n",
            "Soft Drinks              331\n",
            "Meat                     302\n",
            "Breads                   175\n",
            "Hard Drinks              169\n",
            "Others                   130\n",
            "Starchy Foods            122\n",
            "Breakfast                 84\n",
            "Seafood                   42\n",
            "Name: Item_Type, dtype: int64 \n",
            "\n",
            "\n",
            "Item_MRP: float64\n",
            "172.0422    6\n",
            "142.0154    5\n",
            "110.1544    5\n",
            "107.4622    5\n",
            "143.2154    5\n",
            "           ..\n",
            "187.3240    1\n",
            "162.6552    1\n",
            "241.3170    1\n",
            "129.1652    1\n",
            "95.7410     1\n",
            "Name: Item_MRP, Length: 4861, dtype: int64 \n",
            "\n",
            "\n",
            "Outlet_Identifier: object\n",
            "OUT027    723\n",
            "OUT035    709\n",
            "OUT018    704\n",
            "OUT045    699\n",
            "OUT017    698\n",
            "OUT046    695\n",
            "OUT013    689\n",
            "OUT049    676\n",
            "OUT010    415\n",
            "OUT019    384\n",
            "Name: Outlet_Identifier, dtype: int64 \n",
            "\n",
            "\n",
            "Outlet_Establishment_Year: int64\n",
            "1985    1107\n",
            "2004     709\n",
            "2009     704\n",
            "2002     699\n",
            "2007     698\n",
            "1997     695\n",
            "1987     689\n",
            "1999     676\n",
            "1998     415\n",
            "Name: Outlet_Establishment_Year, dtype: int64 \n",
            "\n",
            "\n",
            "Outlet_Size: object\n",
            "Medium    2103\n",
            "NaN       1812\n",
            "Small     1788\n",
            "High       689\n",
            "Name: Outlet_Size, dtype: int64 \n",
            "\n",
            "\n",
            "Outlet_Location_Type: object\n",
            "Tier 3    2531\n",
            "Tier 2    2106\n",
            "Tier 1    1755\n",
            "Name: Outlet_Location_Type, dtype: int64 \n",
            "\n",
            "\n",
            "Outlet_Type: object\n",
            "Supermarket Type1    4166\n",
            "Grocery Store         799\n",
            "Supermarket Type3     723\n",
            "Supermarket Type2     704\n",
            "Name: Outlet_Type, dtype: int64 \n",
            "\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Create a preprocessing object to prepare the dataset for Machine Learning"
      ],
      "metadata": {
        "id": "jc0_oalgyBfP"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#instantiate the selectors to for numeric and categorical data types\n",
        "cat_selector = make_column_selector(dtype_include='object')\n",
        "cat_selector(X_train)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "RyIzb-2cL2zM",
        "outputId": "065a9c0f-079b-483d-80c8-bbebc9e9b716"
      },
      "execution_count": 57,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "['Item_Fat_Content',\n",
              " 'Item_Type',\n",
              " 'Outlet_Identifier',\n",
              " 'Outlet_Size',\n",
              " 'Outlet_Location_Type',\n",
              " 'Outlet_Type']"
            ]
          },
          "metadata": {},
          "execution_count": 57
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "num_selector = make_column_selector(dtype_include='number')\n",
        "num_selector(X_train)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "c7uCgHfUMVcT",
        "outputId": "0ee1c0e1-11ca-4f4f-8e8d-38e6586ae860"
      },
      "execution_count": 58,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "['Item_Weight', 'Item_Visibility', 'Item_MRP', 'Outlet_Establishment_Year']"
            ]
          },
          "metadata": {},
          "execution_count": 58
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# make imputers (will use 'most_frequent' for categorical data, 'mean' for numeric data)\n",
        "freq_imputer = SimpleImputer(strategy = 'most_frequent')\n",
        "mean_imputer = SimpleImputer(strategy = 'mean')"
      ],
      "metadata": {
        "id": "4G9KvVd4ysEV"
      },
      "execution_count": 59,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# make scaler for numeric columns (numeric and ordinal data needs to be scaled)\n",
        "scaler = StandardScaler()"
      ],
      "metadata": {
        "id": "VLWOeWKbyxan"
      },
      "execution_count": 60,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# make scaler for categorical columns\n",
        "ohe = OneHotEncoder(handle_unknown = 'ignore',\n",
        "                    sparse_output = False)"
      ],
      "metadata": {
        "id": "QLFZQ_S_y0n4"
      },
      "execution_count": 61,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# make numeric pipeline\n",
        "numeric_pipe = make_pipeline(mean_imputer, scaler)\n",
        "numeric_pipe"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 126
        },
        "id": "CV-AYun6y4fd",
        "outputId": "cf4e42c8-7c5f-4856-bc16-b894c72cff99"
      },
      "execution_count": 62,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Pipeline(steps=[('simpleimputer', SimpleImputer()),\n",
              "                ('standardscaler', StandardScaler())])"
            ],
            "text/html": [
              "<style>#sk-container-id-1 {color: black;background-color: white;}#sk-container-id-1 pre{padding: 0;}#sk-container-id-1 div.sk-toggleable {background-color: white;}#sk-container-id-1 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-1 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-1 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-1 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-1 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-1 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-1 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-1 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-1 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-1 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-1 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-1 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-1 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-1 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-1 div.sk-item {position: relative;z-index: 1;}#sk-container-id-1 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-1 div.sk-item::before, #sk-container-id-1 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-1 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-1 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-1 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-1 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-1 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-1 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-1 div.sk-label-container {text-align: center;}#sk-container-id-1 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-1 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-1\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>Pipeline(steps=[(&#x27;simpleimputer&#x27;, SimpleImputer()),\n",
              "                (&#x27;standardscaler&#x27;, StandardScaler())])</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item sk-dashed-wrapped\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-1\" type=\"checkbox\" ><label for=\"sk-estimator-id-1\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">Pipeline</label><div class=\"sk-toggleable__content\"><pre>Pipeline(steps=[(&#x27;simpleimputer&#x27;, SimpleImputer()),\n",
              "                (&#x27;standardscaler&#x27;, StandardScaler())])</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-2\" type=\"checkbox\" ><label for=\"sk-estimator-id-2\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">SimpleImputer</label><div class=\"sk-toggleable__content\"><pre>SimpleImputer()</pre></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-3\" type=\"checkbox\" ><label for=\"sk-estimator-id-3\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">StandardScaler</label><div class=\"sk-toggleable__content\"><pre>StandardScaler()</pre></div></div></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 62
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# make categorical pipeline\n",
        "categorical_pipe = make_pipeline(freq_imputer, ohe)\n",
        "categorical_pipe"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 126
        },
        "id": "zLHUXmcvy7BH",
        "outputId": "3daaa1a4-9122-443c-955d-f858b5fa33e9"
      },
      "execution_count": 63,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Pipeline(steps=[('simpleimputer', SimpleImputer(strategy='most_frequent')),\n",
              "                ('onehotencoder',\n",
              "                 OneHotEncoder(handle_unknown='ignore', sparse_output=False))])"
            ],
            "text/html": [
              "<style>#sk-container-id-2 {color: black;background-color: white;}#sk-container-id-2 pre{padding: 0;}#sk-container-id-2 div.sk-toggleable {background-color: white;}#sk-container-id-2 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-2 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-2 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-2 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-2 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-2 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-2 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-2 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-2 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-2 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-2 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-2 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-2 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-2 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-2 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-2 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-2 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-2 div.sk-item {position: relative;z-index: 1;}#sk-container-id-2 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-2 div.sk-item::before, #sk-container-id-2 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-2 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-2 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-2 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-2 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-2 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-2 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-2 div.sk-label-container {text-align: center;}#sk-container-id-2 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-2 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-2\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>Pipeline(steps=[(&#x27;simpleimputer&#x27;, SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                (&#x27;onehotencoder&#x27;,\n",
              "                 OneHotEncoder(handle_unknown=&#x27;ignore&#x27;, sparse_output=False))])</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item sk-dashed-wrapped\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-4\" type=\"checkbox\" ><label for=\"sk-estimator-id-4\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">Pipeline</label><div class=\"sk-toggleable__content\"><pre>Pipeline(steps=[(&#x27;simpleimputer&#x27;, SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                (&#x27;onehotencoder&#x27;,\n",
              "                 OneHotEncoder(handle_unknown=&#x27;ignore&#x27;, sparse_output=False))])</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-5\" type=\"checkbox\" ><label for=\"sk-estimator-id-5\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">SimpleImputer</label><div class=\"sk-toggleable__content\"><pre>SimpleImputer(strategy=&#x27;most_frequent&#x27;)</pre></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-6\" type=\"checkbox\" ><label for=\"sk-estimator-id-6\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">OneHotEncoder</label><div class=\"sk-toggleable__content\"><pre>OneHotEncoder(handle_unknown=&#x27;ignore&#x27;, sparse_output=False)</pre></div></div></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 63
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# make tuples for column transformer\n",
        "numeric_tuple = (numeric_pipe, num_selector)\n",
        "categorical_tuple = (categorical_pipe, cat_selector)"
      ],
      "metadata": {
        "id": "CTmHydkgy-_L"
      },
      "execution_count": 64,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# create preprocessor object (column transformer)\n",
        "preprocessor = make_column_transformer(numeric_tuple, categorical_tuple)\n",
        "preprocessor"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 151
        },
        "id": "hVVQ6lU6y_TX",
        "outputId": "61d30efb-728a-4a0f-a0a6-3cbbbe3017dd"
      },
      "execution_count": 65,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "ColumnTransformer(transformers=[('pipeline-1',\n",
              "                                 Pipeline(steps=[('simpleimputer',\n",
              "                                                  SimpleImputer()),\n",
              "                                                 ('standardscaler',\n",
              "                                                  StandardScaler())]),\n",
              "                                 <sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20>),\n",
              "                                ('pipeline-2',\n",
              "                                 Pipeline(steps=[('simpleimputer',\n",
              "                                                  SimpleImputer(strategy='most_frequent')),\n",
              "                                                 ('onehotencoder',\n",
              "                                                  OneHotEncoder(handle_unknown='ignore',\n",
              "                                                                sparse_output=False))]),\n",
              "                                 <sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400>)])"
            ],
            "text/html": [
              "<style>#sk-container-id-3 {color: black;background-color: white;}#sk-container-id-3 pre{padding: 0;}#sk-container-id-3 div.sk-toggleable {background-color: white;}#sk-container-id-3 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-3 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-3 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-3 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-3 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-3 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-3 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-3 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-3 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-3 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-3 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-3 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-3 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-3 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-3 div.sk-item {position: relative;z-index: 1;}#sk-container-id-3 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-3 div.sk-item::before, #sk-container-id-3 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-3 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-3 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-3 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-3 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-3 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-3 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-3 div.sk-label-container {text-align: center;}#sk-container-id-3 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-3 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-3\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>ColumnTransformer(transformers=[(&#x27;pipeline-1&#x27;,\n",
              "                                 Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                  SimpleImputer()),\n",
              "                                                 (&#x27;standardscaler&#x27;,\n",
              "                                                  StandardScaler())]),\n",
              "                                 &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;),\n",
              "                                (&#x27;pipeline-2&#x27;,\n",
              "                                 Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                  SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                                                 (&#x27;onehotencoder&#x27;,\n",
              "                                                  OneHotEncoder(handle_unknown=&#x27;ignore&#x27;,\n",
              "                                                                sparse_output=False))]),\n",
              "                                 &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;)])</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item sk-dashed-wrapped\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-7\" type=\"checkbox\" ><label for=\"sk-estimator-id-7\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">ColumnTransformer</label><div class=\"sk-toggleable__content\"><pre>ColumnTransformer(transformers=[(&#x27;pipeline-1&#x27;,\n",
              "                                 Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                  SimpleImputer()),\n",
              "                                                 (&#x27;standardscaler&#x27;,\n",
              "                                                  StandardScaler())]),\n",
              "                                 &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;),\n",
              "                                (&#x27;pipeline-2&#x27;,\n",
              "                                 Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                  SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                                                 (&#x27;onehotencoder&#x27;,\n",
              "                                                  OneHotEncoder(handle_unknown=&#x27;ignore&#x27;,\n",
              "                                                                sparse_output=False))]),\n",
              "                                 &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;)])</pre></div></div></div><div class=\"sk-parallel\"><div class=\"sk-parallel-item\"><div class=\"sk-item\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-8\" type=\"checkbox\" ><label for=\"sk-estimator-id-8\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">pipeline-1</label><div class=\"sk-toggleable__content\"><pre>&lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-9\" type=\"checkbox\" ><label for=\"sk-estimator-id-9\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">SimpleImputer</label><div class=\"sk-toggleable__content\"><pre>SimpleImputer()</pre></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-10\" type=\"checkbox\" ><label for=\"sk-estimator-id-10\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">StandardScaler</label><div class=\"sk-toggleable__content\"><pre>StandardScaler()</pre></div></div></div></div></div></div></div></div><div class=\"sk-parallel-item\"><div class=\"sk-item\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-11\" type=\"checkbox\" ><label for=\"sk-estimator-id-11\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">pipeline-2</label><div class=\"sk-toggleable__content\"><pre>&lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-12\" type=\"checkbox\" ><label for=\"sk-estimator-id-12\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">SimpleImputer</label><div class=\"sk-toggleable__content\"><pre>SimpleImputer(strategy=&#x27;most_frequent&#x27;)</pre></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-13\" type=\"checkbox\" ><label for=\"sk-estimator-id-13\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">OneHotEncoder</label><div class=\"sk-toggleable__content\"><pre>OneHotEncoder(handle_unknown=&#x27;ignore&#x27;, sparse_output=False)</pre></div></div></div></div></div></div></div></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 65
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "type(preprocessor)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "zrAfDGAv1wAU",
        "outputId": "da6561e2-0a50-4365-9a3b-68546e178618"
      },
      "execution_count": 66,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "sklearn.compose._column_transformer.ColumnTransformer"
            ]
          },
          "metadata": {},
          "execution_count": 66
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Maching Learning - Training the Models"
      ],
      "metadata": {
        "id": "d9cgg89i04QL"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "from sklearn.linear_model import LinearRegression\n",
        "from sklearn.metrics import r2_score, mean_squared_error, mean_absolute_error"
      ],
      "metadata": {
        "id": "KHKpBHf46sLm"
      },
      "execution_count": 67,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "## Make and fit model\n",
        "linreg_pipe = make_pipeline(preprocessor,LinearRegression())\n",
        "linreg_pipe.fit(X_train, y_train)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 226
        },
        "id": "OzXrN1zn05Qi",
        "outputId": "d772c846-2085-4f4c-d7f4-cb21368c49ef"
      },
      "execution_count": 68,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Pipeline(steps=[('columntransformer',\n",
              "                 ColumnTransformer(transformers=[('pipeline-1',\n",
              "                                                  Pipeline(steps=[('simpleimputer',\n",
              "                                                                   SimpleImputer()),\n",
              "                                                                  ('standardscaler',\n",
              "                                                                   StandardScaler())]),\n",
              "                                                  <sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20>),\n",
              "                                                 ('pipeline-2',\n",
              "                                                  Pipeline(steps=[('simpleimputer',\n",
              "                                                                   SimpleImputer(strategy='most_frequent')),\n",
              "                                                                  ('onehotencoder',\n",
              "                                                                   OneHotEncoder(handle_unknown='ignore',\n",
              "                                                                                 sparse_output=False))]),\n",
              "                                                  <sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400>)])),\n",
              "                ('linearregression', LinearRegression())])"
            ],
            "text/html": [
              "<style>#sk-container-id-4 {color: black;background-color: white;}#sk-container-id-4 pre{padding: 0;}#sk-container-id-4 div.sk-toggleable {background-color: white;}#sk-container-id-4 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-4 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-4 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-4 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-4 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-4 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-4 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-4 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-4 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-4 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-4 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-4 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-4 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-4 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-4 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-4 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-4 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-4 div.sk-item {position: relative;z-index: 1;}#sk-container-id-4 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-4 div.sk-item::before, #sk-container-id-4 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-4 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-4 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-4 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-4 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-4 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-4 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-4 div.sk-label-container {text-align: center;}#sk-container-id-4 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-4 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-4\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>Pipeline(steps=[(&#x27;columntransformer&#x27;,\n",
              "                 ColumnTransformer(transformers=[(&#x27;pipeline-1&#x27;,\n",
              "                                                  Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                                   SimpleImputer()),\n",
              "                                                                  (&#x27;standardscaler&#x27;,\n",
              "                                                                   StandardScaler())]),\n",
              "                                                  &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;),\n",
              "                                                 (&#x27;pipeline-2&#x27;,\n",
              "                                                  Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                                   SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                                                                  (&#x27;onehotencoder&#x27;,\n",
              "                                                                   OneHotEncoder(handle_unknown=&#x27;ignore&#x27;,\n",
              "                                                                                 sparse_output=False))]),\n",
              "                                                  &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;)])),\n",
              "                (&#x27;linearregression&#x27;, LinearRegression())])</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item sk-dashed-wrapped\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-14\" type=\"checkbox\" ><label for=\"sk-estimator-id-14\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">Pipeline</label><div class=\"sk-toggleable__content\"><pre>Pipeline(steps=[(&#x27;columntransformer&#x27;,\n",
              "                 ColumnTransformer(transformers=[(&#x27;pipeline-1&#x27;,\n",
              "                                                  Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                                   SimpleImputer()),\n",
              "                                                                  (&#x27;standardscaler&#x27;,\n",
              "                                                                   StandardScaler())]),\n",
              "                                                  &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;),\n",
              "                                                 (&#x27;pipeline-2&#x27;,\n",
              "                                                  Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                                   SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                                                                  (&#x27;onehotencoder&#x27;,\n",
              "                                                                   OneHotEncoder(handle_unknown=&#x27;ignore&#x27;,\n",
              "                                                                                 sparse_output=False))]),\n",
              "                                                  &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;)])),\n",
              "                (&#x27;linearregression&#x27;, LinearRegression())])</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item sk-dashed-wrapped\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-15\" type=\"checkbox\" ><label for=\"sk-estimator-id-15\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">columntransformer: ColumnTransformer</label><div class=\"sk-toggleable__content\"><pre>ColumnTransformer(transformers=[(&#x27;pipeline-1&#x27;,\n",
              "                                 Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                  SimpleImputer()),\n",
              "                                                 (&#x27;standardscaler&#x27;,\n",
              "                                                  StandardScaler())]),\n",
              "                                 &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;),\n",
              "                                (&#x27;pipeline-2&#x27;,\n",
              "                                 Pipeline(steps=[(&#x27;simpleimputer&#x27;,\n",
              "                                                  SimpleImputer(strategy=&#x27;most_frequent&#x27;)),\n",
              "                                                 (&#x27;onehotencoder&#x27;,\n",
              "                                                  OneHotEncoder(handle_unknown=&#x27;ignore&#x27;,\n",
              "                                                                sparse_output=False))]),\n",
              "                                 &lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;)])</pre></div></div></div><div class=\"sk-parallel\"><div class=\"sk-parallel-item\"><div class=\"sk-item\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-16\" type=\"checkbox\" ><label for=\"sk-estimator-id-16\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">pipeline-1</label><div class=\"sk-toggleable__content\"><pre>&lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879875e20&gt;</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-17\" type=\"checkbox\" ><label for=\"sk-estimator-id-17\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">SimpleImputer</label><div class=\"sk-toggleable__content\"><pre>SimpleImputer()</pre></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-18\" type=\"checkbox\" ><label for=\"sk-estimator-id-18\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">StandardScaler</label><div class=\"sk-toggleable__content\"><pre>StandardScaler()</pre></div></div></div></div></div></div></div></div><div class=\"sk-parallel-item\"><div class=\"sk-item\"><div class=\"sk-label-container\"><div class=\"sk-label sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-19\" type=\"checkbox\" ><label for=\"sk-estimator-id-19\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">pipeline-2</label><div class=\"sk-toggleable__content\"><pre>&lt;sklearn.compose._column_transformer.make_column_selector object at 0x7f9879871400&gt;</pre></div></div></div><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-serial\"><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-20\" type=\"checkbox\" ><label for=\"sk-estimator-id-20\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">SimpleImputer</label><div class=\"sk-toggleable__content\"><pre>SimpleImputer(strategy=&#x27;most_frequent&#x27;)</pre></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-21\" type=\"checkbox\" ><label for=\"sk-estimator-id-21\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">OneHotEncoder</label><div class=\"sk-toggleable__content\"><pre>OneHotEncoder(handle_unknown=&#x27;ignore&#x27;, sparse_output=False)</pre></div></div></div></div></div></div></div></div></div></div><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-22\" type=\"checkbox\" ><label for=\"sk-estimator-id-22\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">LinearRegression</label><div class=\"sk-toggleable__content\"><pre>LinearRegression()</pre></div></div></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 68
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# transform both training and test data\n",
        "X_train_processed = preprocessor.transform(X_train)\n",
        "X_test_processed = preprocessor.transform(X_test)"
      ],
      "metadata": {
        "id": "fumzKKOh1Bt8"
      },
      "execution_count": 69,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print(np.isnan(X_train_processed).sum().sum(), 'missing values in training data')\n",
        "print(np.isnan(X_test_processed).sum().sum(), 'missing values in test data')\n",
        "print()\n",
        "print('All data in X_train_processed are', X_train_processed.dtype)\n",
        "print('All data in X_test_processed are', X_test_processed.dtype)\n",
        "print()\n",
        "print('Shape of data is', X_train_processed.shape)\n",
        "print(X_train_processed)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "ELMBnbeu1EZF",
        "outputId": "4f830a41-a5c0-47c6-dd92-1c2235880a96"
      },
      "execution_count": 70,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0 missing values in training data\n",
            "0 missing values in test data\n",
            "\n",
            "All data in X_train_processed are float64\n",
            "All data in X_test_processed are float64\n",
            "\n",
            "Shape of data is (6392, 42)\n",
            "[[ 0.81724868 -0.71277507  1.82810922 ...  0.          1.\n",
            "   0.        ]\n",
            " [ 0.5563395  -1.29105225  0.60336888 ...  0.          1.\n",
            "   0.        ]\n",
            " [-0.13151196  1.81331864  0.24454056 ...  1.          0.\n",
            "   0.        ]\n",
            " ...\n",
            " [ 1.11373638 -0.92052713  1.52302674 ...  1.          0.\n",
            "   0.        ]\n",
            " [ 1.76600931 -0.2277552  -0.38377708 ...  1.          0.\n",
            "   0.        ]\n",
            " [ 0.81724868 -0.95867683 -0.73836105 ...  1.          0.\n",
            "   0.        ]]\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# create model predictions on training and testing data\n",
        "linreg_train = linreg_pipe.predict(X_train)\n",
        "linreg_test = linreg_pipe.predict(X_test)"
      ],
      "metadata": {
        "id": "xvYQBbvl7u4t"
      },
      "execution_count": 71,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "#Function to Evaluate Model"
      ],
      "metadata": {
        "id": "AnEVZlee8VWW"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "def evaluate_model(y_true, y_pred, split='training'):\n",
        "    \n",
        "  r2 = r2_score(y_true,y_pred)\n",
        "  mae = mean_absolute_error(y_true,y_pred)\n",
        "  mse = mean_squared_error(y_true, y_pred)\n",
        "  rmse = mean_squared_error(y_true,y_pred,squared=False)\n",
        "\n",
        "  \n",
        "  print(f'Results for {split} data:')\n",
        "  print(f\"  - R^2 = {round(r2,3)}\")\n",
        "  print(f\"  - MAE = {round(mae,3)}\")\n",
        "  print(f\"  - MSE = {round(mse,3)}\")\n",
        "  print(f\"  - RMSE = {round(rmse,3)}\")\n",
        "  print()"
      ],
      "metadata": {
        "id": "kcpV6cPe8BrJ"
      },
      "execution_count": 72,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Linear Regression Model r^2"
      ],
      "metadata": {
        "id": "6FZZ_UXn1M7u"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "## Evaluate model's performance\n",
        "evaluate_model(y_train, linreg_train,split='training')\n",
        "evaluate_model(y_test, linreg_test,split='testing')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "gJwSV5ug9LG0",
        "outputId": "1561d941-7287-442d-a29f-684b07ac398a"
      },
      "execution_count": 73,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Results for training data:\n",
            "  - R^2 = 0.562\n",
            "  - MAE = 847.128\n",
            "  - MSE = 1297558.183\n",
            "  - RMSE = 1139.104\n",
            "\n",
            "Results for testing data:\n",
            "  - R^2 = 0.567\n",
            "  - MAE = 804.118\n",
            "  - MSE = 1194347.614\n",
            "  - RMSE = 1092.862\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "1. This model performs well on both the training and test set\n",
        "2. This model can account for about 57% of the variation in the test data "
      ],
      "metadata": {
        "id": "D7UO9mcJ92X8"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Linear Regression Model rmse"
      ],
      "metadata": {
        "id": "8p2Z-9uK-WZD"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "from sklearn.tree import DecisionTreeRegressor\n",
        "from sklearn.ensemble import RandomForestRegressor"
      ],
      "metadata": {
        "id": "OMcP6yjo-SHt"
      },
      "execution_count": 74,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "## Make and fit model\n",
        "linreg_tree_pipe = make_pipeline(preprocessor,DecisionTreeRegressor(random_state = 42))\n",
        "linreg_tree_pipe.fit(X_train, y_train)\n",
        "\n",
        "## Get predictions for training and test data\n",
        "linreg_train = linreg_tree_pipe.predict(X_train)\n",
        "linreg_test = linreg_tree_pipe.predict(X_test)"
      ],
      "metadata": {
        "id": "UpDL7pv3_L-g"
      },
      "execution_count": 75,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "## Evaluate model's performance\n",
        "evaluate_model(y_train, linreg_train,split='training')\n",
        "evaluate_model(y_test, linreg_test,split='testing')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "XBDgFjnp_khl",
        "outputId": "dbcbf0eb-38dd-4772-90f1-3521c4d8f3b3"
      },
      "execution_count": 76,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Results for training data:\n",
            "  - R^2 = 1.0\n",
            "  - MAE = 0.0\n",
            "  - MSE = 0.0\n",
            "  - RMSE = 0.0\n",
            "\n",
            "Results for testing data:\n",
            "  - R^2 = 0.184\n",
            "  - MAE = 1044.998\n",
            "  - MSE = 2251075.165\n",
            "  - RMSE = 1500.358\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# UnTuned Depth for Decision Tree Regressor Model\n",
        "- This rmse model performs well on testing data but is not enough data shown for training set\n",
        "- This model is underfit and needs tuning"
      ],
      "metadata": {
        "id": "9ye_9tFsAYON"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#create a range of max_depth values\n",
        "depths = range(1, linreg_tree_pipe['decisiontreeregressor'].get_depth())\n",
        "\n",
        "#create a dataframe to store train and test scores.\n",
        "scores = pd.DataFrame(columns=['Train', 'Test'], index=depths)\n",
        "\n",
        "#loop over the values in depths\n",
        "for n in depths:\n",
        "  #fit a new model with max_depth\n",
        "  tree = DecisionTreeRegressor(random_state = 42, max_depth=n)\n",
        "\n",
        "  #put the model into a pipeline\n",
        "  tree_pipe = make_pipeline(preprocessor, tree)\n",
        "  \n",
        "  #fit the model\n",
        "  tree_pipe.fit(X_train, y_train)\n",
        "  \n",
        "  #create prediction arrays\n",
        "  train_pred = tree_pipe.predict(X_train)\n",
        "  test_pred = tree_pipe.predict(X_test)\n",
        "  \n",
        "  #evaluate the model using R2 Score\n",
        "  train_r2score = r2_score(y_train, train_pred)\n",
        "  test_r2score = r2_score(y_test, test_pred)\n",
        "  \n",
        "  #store the scores in the scores dataframe\n",
        "  scores.loc[n, 'Train'] = train_r2score\n",
        "  scores.loc[n, 'Test'] = test_r2score"
      ],
      "metadata": {
        "id": "XZeFcneeB4Lh"
      },
      "execution_count": 77,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#Scores from Decision Tree\n",
        "scores"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 1000
        },
        "id": "9YIysNblC1Fe",
        "outputId": "ff231a39-6799-483c-f3bb-4885fbf4c6ea"
      },
      "execution_count": 78,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "       Train      Test\n",
              "1   0.237797  0.229683\n",
              "2   0.431641  0.433778\n",
              "3   0.524218  0.524222\n",
              "4   0.582625  0.584005\n",
              "5    0.60394   0.59471\n",
              "6   0.615161  0.582274\n",
              "7   0.626843  0.576476\n",
              "8   0.643832  0.557416\n",
              "9   0.665649  0.541598\n",
              "10  0.685258  0.530134\n",
              "11  0.708597  0.514666\n",
              "12  0.734539  0.479719\n",
              "13  0.762242  0.437062\n",
              "14  0.791684  0.407151\n",
              "15  0.819276  0.377502\n",
              "16  0.845279  0.348912\n",
              "17  0.871177  0.321197\n",
              "18  0.895447  0.303587\n",
              "19  0.916894   0.27895\n",
              "20  0.935004  0.274718\n",
              "21  0.950783  0.258998\n",
              "22  0.960808  0.235135\n",
              "23  0.970288  0.229486\n",
              "24  0.976619  0.229452\n",
              "25  0.982187  0.216919\n",
              "26  0.986464  0.199066\n",
              "27  0.990518  0.209218\n",
              "28  0.992775   0.21503\n",
              "29  0.994387  0.201153\n",
              "30  0.995948  0.189659\n",
              "31  0.997014  0.186591\n",
              "32  0.997695  0.187435\n",
              "33  0.998543  0.194617\n",
              "34  0.999067  0.200533\n",
              "35  0.999538  0.185491\n",
              "36  0.999794  0.207307\n",
              "37  0.999971  0.197974\n",
              "38  0.999996  0.202866\n",
              "39  0.999999  0.201208"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-84c8710e-34dd-4262-8426-ca64ae40cbd2\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Train</th>\n",
              "      <th>Test</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>0.237797</td>\n",
              "      <td>0.229683</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>0.431641</td>\n",
              "      <td>0.433778</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>0.524218</td>\n",
              "      <td>0.524222</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>0.582625</td>\n",
              "      <td>0.584005</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>0.60394</td>\n",
              "      <td>0.59471</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>6</th>\n",
              "      <td>0.615161</td>\n",
              "      <td>0.582274</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>7</th>\n",
              "      <td>0.626843</td>\n",
              "      <td>0.576476</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>8</th>\n",
              "      <td>0.643832</td>\n",
              "      <td>0.557416</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9</th>\n",
              "      <td>0.665649</td>\n",
              "      <td>0.541598</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>10</th>\n",
              "      <td>0.685258</td>\n",
              "      <td>0.530134</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>11</th>\n",
              "      <td>0.708597</td>\n",
              "      <td>0.514666</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>12</th>\n",
              "      <td>0.734539</td>\n",
              "      <td>0.479719</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>13</th>\n",
              "      <td>0.762242</td>\n",
              "      <td>0.437062</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>14</th>\n",
              "      <td>0.791684</td>\n",
              "      <td>0.407151</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>15</th>\n",
              "      <td>0.819276</td>\n",
              "      <td>0.377502</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>16</th>\n",
              "      <td>0.845279</td>\n",
              "      <td>0.348912</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>17</th>\n",
              "      <td>0.871177</td>\n",
              "      <td>0.321197</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>18</th>\n",
              "      <td>0.895447</td>\n",
              "      <td>0.303587</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>19</th>\n",
              "      <td>0.916894</td>\n",
              "      <td>0.27895</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>20</th>\n",
              "      <td>0.935004</td>\n",
              "      <td>0.274718</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>21</th>\n",
              "      <td>0.950783</td>\n",
              "      <td>0.258998</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>22</th>\n",
              "      <td>0.960808</td>\n",
              "      <td>0.235135</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>23</th>\n",
              "      <td>0.970288</td>\n",
              "      <td>0.229486</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>24</th>\n",
              "      <td>0.976619</td>\n",
              "      <td>0.229452</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25</th>\n",
              "      <td>0.982187</td>\n",
              "      <td>0.216919</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>26</th>\n",
              "      <td>0.986464</td>\n",
              "      <td>0.199066</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>27</th>\n",
              "      <td>0.990518</td>\n",
              "      <td>0.209218</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>28</th>\n",
              "      <td>0.992775</td>\n",
              "      <td>0.21503</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>29</th>\n",
              "      <td>0.994387</td>\n",
              "      <td>0.201153</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>30</th>\n",
              "      <td>0.995948</td>\n",
              "      <td>0.189659</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>31</th>\n",
              "      <td>0.997014</td>\n",
              "      <td>0.186591</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>32</th>\n",
              "      <td>0.997695</td>\n",
              "      <td>0.187435</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>33</th>\n",
              "      <td>0.998543</td>\n",
              "      <td>0.194617</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>34</th>\n",
              "      <td>0.999067</td>\n",
              "      <td>0.200533</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>35</th>\n",
              "      <td>0.999538</td>\n",
              "      <td>0.185491</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>36</th>\n",
              "      <td>0.999794</td>\n",
              "      <td>0.207307</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>37</th>\n",
              "      <td>0.999971</td>\n",
              "      <td>0.197974</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>38</th>\n",
              "      <td>0.999996</td>\n",
              "      <td>0.202866</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>39</th>\n",
              "      <td>0.999999</td>\n",
              "      <td>0.201208</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-84c8710e-34dd-4262-8426-ca64ae40cbd2')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-84c8710e-34dd-4262-8426-ca64ae40cbd2 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-84c8710e-34dd-4262-8426-ca64ae40cbd2');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 78
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "### Plotting Scores for Decision Tree Train & Test Visually"
      ],
      "metadata": {
        "id": "wCOxEgm_DDYQ"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#plot the scores to visually determine the best max_depth\n",
        "plt.plot(depths, scores['Train'], label = 'train')\n",
        "plt.plot(depths, scores['Test'], label = 'test')\n",
        "plt.ylabel('R2 Scores')\n",
        "plt.xlabel('Max Depths')\n",
        "plt.legend()\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 449
        },
        "id": "hslMP7JRB2Zc",
        "outputId": "11912d8b-7c7c-4e61-a703-21fe7320efd5"
      },
      "execution_count": 79,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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gnuc1xMtWrSIDz74gDZt2hRbX67n15CTdO7c2XjwwQeLnjudTiMqKspITEy0sKqK8eyzzxpxcXFWl1HhAOPbb78teu5yuYyIiAjj9ddfL1p36NAhw+FwGF9++aUFFZavE4/XMAxjwIABxnXXXWdJPRUtNTXVAIy5c+cahmGeSy8vL+Prr78u2mbdunUGYMybN8+qMsvFicdqGIZxySWXGI888oh1RVWgWrVqGR999JFbn9PjHT1ew3DP85qZmWk0bdrUmDVrVrHjK+/zqys3J8jPz2fJkiV07969aJ2Hhwfdu3dn3rx5FlZWcTZt2kRUVBSNGjWiX79+7Ny50+qSKty2bdtITk4udp6Dg4OJj4932/MMMGfOHOrWrUuzZs24//772b9/v9UllYv09HQAQkNDAViyZAkFBQXFzm/z5s2pX79+tT+/Jx7rUZ9//jlhYWG0atWKESNGkJOTY0V55cbpdDJ58mSys7Pp0qWLW59TOPl4j3K38/rggw9y9dVXFzuPUP7/z9a4iTPPJi0tDafTSXh4eLH14eHhrF+/3qKqKk58fDwTJ06kWbNmJCUl8fzzz3PRRRexevVqAgMDrS6vwiQnJwOc8jwffc3d9OrVixtvvJGGDRuyZcsW/vOf/9C7d2/mzZuH3W63urwyc7lcPProo3Tr1o1WrVoB5vn19vYmJCSk2LbV/fye6lgBbr/9dho0aEBUVBQrV67kySefZMOGDUybNs3Castm1apVdOnShdzcXAICAvj2229p2bIly5cvd8tzerrjBfc6rwCTJ09m6dKlLFq06KTXyvv/WYWbGq53795Fj9u0aUN8fDwNGjTgq6++4u6777awMilvt956a9Hj1q1b06ZNGxo3bsycOXO44oorLKzs3Dz44IOsXr3abdqKncnpjvXee+8tety6dWsiIyO54oor2LJlC40bN67sMs9Js2bNWL58Oenp6UydOpUBAwYwd+5cq8uqMKc73pYtW7rVed21axePPPIIs2bNwsfHp8I/T7elThAWFobdbj+phXZKSgoREREWVVV5QkJCOO+889i8ebPVpVSoo+eypp5ngEaNGhEWFlatz/XQoUP58ccf+eOPP6hXr17R+oiICPLz8zl06FCx7avz+T3dsZ5KfHw8QLU8t97e3jRp0oQOHTqQmJhIXFwc77zzjlueUzj98Z5KdT6vS5YsITU1lfbt2+Pp6Ymnpydz587l3XffxdPTk/Dw8HI9vwo3J/D29qZDhw7Mnj27aJ3L5WL27NnF7oO6q6ysLLZs2UJkZKTVpVSohg0bEhERUew8Z2RksGDBghpxngF2797N/v37q+W5NgyDoUOH8u233/L777/TsGHDYq936NABLy+vYud3w4YN7Ny5s9qd37Md66ksX74coFqe2xO5XC7y8vLc6pyeydHjPZXqfF6vuOIKVq1axfLly4uWjh070q9fv6LH5Xp+y6f9s3uZPHmy4XA4jIkTJxpr16417r33XiMkJMRITk62urRy99hjjxlz5swxtm3bZvz9999G9+7djbCwMCM1NdXq0s5ZZmamsWzZMmPZsmUGYLz11lvGsmXLjB07dhiGYRivvPKKERISYnz//ffGypUrjeuuu85o2LChcfjwYYsrL5szHW9mZqbx+OOPG/PmzTO2bdtm/Pbbb0b79u2Npk2bGrm5uVaXXmr333+/ERwcbMyZM8dISkoqWnJycoq2GTJkiFG/fn3j999/NxYvXmx06dLF6NKli4VVl83ZjnXz5s3GCy+8YCxevNjYtm2b8f333xuNGjUyLr74YosrL73hw4cbc+fONbZt22asXLnSGD58uGGz2Yxff/3VMAz3OadHnel43em8ns6JvcHK8/wq3JzG6NGjjfr16xve3t5G586djfnz51tdUoXo27evERkZaXh7exvR0dFG3759jc2bN1tdVrn4448/DOCkZcCAAYZhmN3Bn3nmGSM8PNxwOBzGFVdcYWzYsMHaos/BmY43JyfH6NGjh1GnTh3Dy8vLaNCggTF48OBqG9hPdZyAMWHChKJtDh8+bDzwwANGrVq1DD8/P+OGG24wkpKSrCu6jM52rDt37jQuvvhiIzQ01HA4HEaTJk2Mf//730Z6erq1hZfBXXfdZTRo0MDw9vY26tSpY1xxxRVFwcYw3OecHnWm43Wn83o6J4ab8jy/NsMwjDJcYRIRERGpktTmRkRERNyKwo2IiIi4FYUbERERcSsKNyIiIuJWFG5ERETErSjciIiIiFtRuBERERG3onAjIiIibkXhRkSkjGJjYxk1apTVZYjICRRuRKRUBg4ciM1mY8iQISe99uCDD2Kz2Rg4cGCF1jBx4kRsNhs2mw273U6tWrWIj4/nhRdeID09vUI+LyQkpNzfV0QqhsKNiJRaTEwMkydP5vDhw0XrcnNz+eKLL6hfv36l1BAUFERSUhK7d+/mn3/+4d5772XSpEm0bduWvXv3VkoNIlI1KdyISKm1b9+emJgYpk2bVrRu2rRp1K9fn3bt2hXbdubMmVx44YWEhIRQu3ZtrrnmGrZs2VL0+qRJkwgICGDTpk1F6x544AGaN29OTk7OaWuw2WxEREQQGRlJixYtuPvuu/nnn3/IysriiSeeKNrO5XKRmJhIw4YN8fX1JS4ujqlTpxa9PmfOHGw2GzNmzKBNmzb4+PhwwQUXsHr16qLXBw0aRHp6etHVoueee65o/5ycHO666y4CAwOpX78+H374YdFr+fn5DB06lMjISHx8fGjQoAGJiYml+E2LSFko3IhImdx1111MmDCh6Pn48eMZNGjQSdtlZ2eTkJDA4sWLmT17Nh4eHtxwww24XC4A+vfvz1VXXUW/fv0oLCxkxowZfPTRR3z++ef4+fmVqqa6devSr18/pk+fjtPpBCAxMZFJkyYxduxY1qxZw7Bhw7jjjjuYO3dusX3//e9/8+abb7Jo0SLq1KlDnz59KCgooGvXrowaNaroSlFSUhKPP/540X5vvvkmHTt2ZNmyZTzwwAPcf//9bNiwAYB3332X6dOn89VXX7FhwwY+//xzYmNjS3VMIlIG5TJvuYjUGAMGDDCuu+46IzU11XA4HMb27duN7du3Gz4+Psa+ffuM6667zhgwYMBp99+3b58BGKtWrSpad+DAAaNevXrG/fffb4SHhxsvvfTSGWuYMGGCERwcfMrX3n//fQMwUlJSjNzcXMPPz8/4559/im1z9913G7fddpthGIbxxx9/GIAxefLkotf3799v+Pr6GlOmTDnj5zVo0MC44447ip67XC6jbt26xvvvv28YhmE89NBDxuWXX264XK4zHo+IlC9Pi7OViFRTderU4eqrr2bixIkYhsHVV19NWFjYSdtt2rSJkSNHsmDBAtLS0oqu2OzcuZNWrVoBUKtWLT7++GN69uxJ165dGT58eJnrMgwDMG9bbd68mZycHK688spi2+Tn5590+6xLly5Fj0NDQ2nWrBnr1q076+e1adOm6PHRW2WpqamA2fj6yiuvpFmzZvTq1YtrrrmGHj16lPnYRKRkFG5EpMzuuusuhg4dCsCYMWNOuU2fPn1o0KAB48aNIyoqCpfLRatWrcjPzy+23f/+9z/sdjtJSUlkZ2cTGBhYpprWrVtHUFAQtWvXZuvWrQDMmDGD6OjoYts5HI4yvf+JvLy8ij232WxFAa59+/Zs27aNn3/+md9++41bbrmF7t27F2vzIyLlT21uRKTMevXqRX5+PgUFBfTs2fOk1/fv38+GDRt4+umnueKKK2jRogUHDx48abt//vmHV199lR9++IGAgICiwFRaqampfPHFF1x//fV4eHjQsmVLHA4HO3fupEmTJsWWmJiYYvvOnz+/6PHBgwfZuHEjLVq0AMDb27uoDU9pBQUF0bdvX8aNG8eUKVP45ptvOHDgQJneS0RKRlduRKTM7HZ70a0bu91+0uu1atWidu3afPjhh0RGRrJz586TbjllZmZy55138vDDD9O7d2/q1atHp06d6NOnDzfffPNpP9swDJKTkzEMg0OHDjFv3jxefvllgoODeeWVVwAIDAzk8ccfZ9iwYbhcLi688ELS09P5+++/CQoKYsCAAUXv98ILL1C7dm3Cw8N56qmnCAsL4/rrrwfMwfqysrKYPXs2cXFx+Pn5laix81tvvUVkZCTt2rXDw8ODr7/+moiICI2ZI1LBdOVGRM5JUFAQQUFBp3zNw8ODyZMns2TJElq1asWwYcN4/fXXi23zyCOP4O/vz8svvwxA69atefnll7nvvvvYs2fPaT83IyODyMhIoqOj6dKlCx988AEDBgxg2bJlREZGFm334osv8swzz5CYmEiLFi3o1asXM2bMoGHDhsXe75VXXuGRRx6hQ4cOJCcn88MPP+Dt7Q1A165dGTJkCH379qVOnTq89tprJfrdBAYG8tprr9GxY0c6derE9u3b+emnn/Dw0D+9IhXJZhxtfSciUgPNmTOHyy67jIMHD+qKioib0J8PIiIi4lYUbkRERMSt6LaUiIiIuBVduRERERG3onAjIiIibkXhRkRERNyKwo2IiIi4FYUbERERcSsKNyIiIuJWFG5ERETErSjciIiIiFv5f+T8igEOqbTEAAAAAElFTkSuQmCC\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "### Best Depth for Decision Tree Model"
      ],
      "metadata": {
        "id": "0j903hzPDLZX"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#sort the dataframe by test scores and save the index (k) of the best score\n",
        "best_depth = scores.sort_values(by='Test', ascending=False).index[0]\n",
        "best_depth"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "PiM-_JLbDI2f",
        "outputId": "fdbd4d0f-8ec1-45af-c582-3d800627b7d1"
      },
      "execution_count": 80,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "5"
            ]
          },
          "metadata": {},
          "execution_count": 80
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "### Reevaluating Decision Tree Regressor Model"
      ],
      "metadata": {
        "id": "IG3-rzPADxwx"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "best_tree = DecisionTreeRegressor(random_state = 42, max_depth=best_depth)\n",
        "\n",
        "best_tree_pipe = make_pipeline(preprocessor, best_tree)\n",
        "\n",
        "best_tree_pipe.fit(X_train, y_train)\n",
        "\n",
        "print('Training Scores for High Variance Decision Tree')\n",
        "evaluate_model(y_train, best_tree_pipe.predict(X_train), split = 'training')\n",
        "\n",
        "print('\\n')\n",
        "\n",
        "print('Testing Scores for High Variance Decision Tree')\n",
        "evaluate_model(y_test, best_tree_pipe.predict(X_test), split = 'testing')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "eaYH_ifZDeud",
        "outputId": "f5d772fe-7429-4aa6-e687-171d3a559673"
      },
      "execution_count": 81,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Training Scores for High Variance Decision Tree\n",
            "Results for training data:\n",
            "  - R^2 = 0.604\n",
            "  - MAE = 762.61\n",
            "  - MSE = 1172122.773\n",
            "  - RMSE = 1082.646\n",
            "\n",
            "\n",
            "\n",
            "Testing Scores for High Variance Decision Tree\n",
            "Results for testing data:\n",
            "  - R^2 = 0.595\n",
            "  - MAE = 738.317\n",
            "  - MSE = 1118185.973\n",
            "  - RMSE = 1057.443\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Tuned (Max Depth) Decision Tree Model\n",
        "- This data set shows low bias as it performed well in both training and testing\n",
        "- This represents true low variance as the predictions of training and test scores are accurate"
      ],
      "metadata": {
        "id": "5EGc26ufGW9D"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Fitting a Random Forest Regressor Model"
      ],
      "metadata": {
        "id": "p0jfOmXFjSa8"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#Fitting a Random Forest Regressor Model\n",
        "rainfor_tree_pipe = make_pipeline(preprocessor,RandomForestRegressor(random_state = 42))\n",
        "rainfor_tree_pipe.fit(X_train, y_train)\n",
        "\n",
        "## Get predictions for training and test data\n",
        "linreg_train = rainfor_tree_pipe.predict(X_train)\n",
        "linreg_test = rainfor_tree_pipe.predict(X_test)"
      ],
      "metadata": {
        "id": "PBVWwho4Gskj"
      },
      "execution_count": 82,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "## Evaluate Random Forest Regressor model's performance\n",
        "evaluate_model(y_train, linreg_train,split='training')\n",
        "evaluate_model(y_test, linreg_test,split='testing')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "tEM8ANbLHrFN",
        "outputId": "3db7d5ad-f420-4c84-d114-d4cc602fddd6"
      },
      "execution_count": 83,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Results for training data:\n",
            "  - R^2 = 0.938\n",
            "  - MAE = 296.606\n",
            "  - MSE = 182863.811\n",
            "  - RMSE = 427.626\n",
            "\n",
            "Results for testing data:\n",
            "  - R^2 = 0.56\n",
            "  - MAE = 766.068\n",
            "  - MSE = 1214585.128\n",
            "  - RMSE = 1102.082\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Untuned Random Forest Model Observations\n",
        "- The results improved as far as the regression metrics and the model's performance.\n",
        "-This model still has some bias. \n",
        "- For the R^2 score 56.0% of the variance is explained.\n",
        "- For the MAE the testing data is off by about $766.07"
      ],
      "metadata": {
        "id": "cReLG7koenvB"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "### Creating a Loop to Tune n_estimators for Random Forest Regressor Model"
      ],
      "metadata": {
        "id": "UzRtkZlqkcp2"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#create a range of max_depth values\n",
        "n_estimators = [50, 100, 150, 200, 250, 300, 350, 400]\n",
        "\n",
        "#create a dataframe to store train and test scores.\n",
        "scores = pd.DataFrame(columns=['Train', 'Test'], index=n_estimators)\n",
        "#loop over the values in depths\n",
        "for n in n_estimators:\n",
        "  #fit a new model with max_depth\n",
        "  rf = RandomForestRegressor(random_state = 42, n_estimators=n)\n",
        "\n",
        "  #put the model into a pipeline\n",
        "  rf_pipe = make_pipeline(preprocessor, rf)\n",
        "  \n",
        "  #fit the model\n",
        "  rf_pipe.fit(X_train, y_train)\n",
        "  \n",
        "  #create prediction arrays\n",
        "  train_pred = rf_pipe.predict(X_train)\n",
        "  test_pred = rf_pipe.predict(X_test)\n",
        "  \n",
        "  #evaluate the model using R2 Score\n",
        "  train_r2score = r2_score(y_train, train_pred)\n",
        "  test_r2score = r2_score(y_test, test_pred)\n",
        "  \n",
        "  #store the scores in the scores dataframe\n",
        "  scores.loc[n, 'Train'] = train_r2score\n",
        "  scores.loc[n, 'Test'] = test_r2score\n"
      ],
      "metadata": {
        "id": "RyHsfXqmdMU0"
      },
      "execution_count": 84,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "scores.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 203
        },
        "id": "FCBvNUTyZdfL",
        "outputId": "000de90c-5aeb-43cd-eed2-c0aaed4149ea"
      },
      "execution_count": 86,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "        Train      Test\n",
              "50   0.935491  0.556445\n",
              "100   0.93821   0.55977\n",
              "150  0.939019  0.560826\n",
              "200  0.939439  0.559095\n",
              "250  0.939697  0.560018"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-5f96bb73-b2a8-4395-a7ae-c67964871bac\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Train</th>\n",
              "      <th>Test</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>50</th>\n",
              "      <td>0.935491</td>\n",
              "      <td>0.556445</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>100</th>\n",
              "      <td>0.93821</td>\n",
              "      <td>0.55977</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>150</th>\n",
              "      <td>0.939019</td>\n",
              "      <td>0.560826</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>200</th>\n",
              "      <td>0.939439</td>\n",
              "      <td>0.559095</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>250</th>\n",
              "      <td>0.939697</td>\n",
              "      <td>0.560018</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-5f96bb73-b2a8-4395-a7ae-c67964871bac')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-5f96bb73-b2a8-4395-a7ae-c67964871bac button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-5f96bb73-b2a8-4395-a7ae-c67964871bac');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 86
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "best_estimators = scores.sort_values(by='Test', ascending=False).index[0]\n",
        "best_estimators"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "L7arUiyzZytG",
        "outputId": "b998e014-e778-4a60-e723-0a938f8ce4ad"
      },
      "execution_count": 87,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "150"
            ]
          },
          "metadata": {},
          "execution_count": 87
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [],
      "metadata": {
        "id": "Fc-hr4IodPuP"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "best_rf = RandomForestRegressor(random_state = 42, n_estimators=best_estimators)\n",
        "\n",
        "best_rf_pipe = make_pipeline(preprocessor, best_rf)\n",
        "\n",
        "best_rf_pipe.fit(X_train, y_train)\n",
        "\n",
        "print('Training Scores for High Variance Decision Tree')\n",
        "evaluate_model(y_train, best_rf_pipe.predict(X_train), split = 'training')\n",
        "\n",
        "print('\\n')\n",
        "\n",
        "print('Testing Scores for High Variance Decision Tree')\n",
        "evaluate_model(y_test, best_rf_pipe.predict(X_test), split = 'testing')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "gt1mTcujc6Qk",
        "outputId": "d79baecc-e429-4fa9-bbbe-f5c2c7f5f4dc"
      },
      "execution_count": 88,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Training Scores for High Variance Decision Tree\n",
            "Results for training data:\n",
            "  - R^2 = 0.939\n",
            "  - MAE = 295.985\n",
            "  - MSE = 180470.327\n",
            "  - RMSE = 424.818\n",
            "\n",
            "\n",
            "\n",
            "Testing Scores for High Variance Decision Tree\n",
            "Results for testing data:\n",
            "  - R^2 = 0.561\n",
            "  - MAE = 765.628\n",
            "  - MSE = 1211670.531\n",
            "  - RMSE = 1100.759\n",
            "\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Tuned (n_estimators) Random Forest Model Observations\n",
        "- This model contains bias but has great performance on testing score.\n",
        "- For r^2 sscore of 56% of the variance is explained\n",
        "- The MAE testing score is off about $766"
      ],
      "metadata": {
        "id": "lbwCIAQxq-ta"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Model Decision\n",
        "- I would recommend the UnTuned Depth Decision Tree Regressor Model as it out performed all the other models that showed more bias.\n",
        "- The first model had potential but the rmse testing scores were off.\n",
        "- The linear regression model had closer predictions"
      ],
      "metadata": {
        "id": "Tn6leor7Asbp"
      }
    }
  ]
}