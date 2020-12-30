# python-data-project
[ESILV] Project on Python for Data Analysis

{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "Online Shoppers Dataset",
      "provenance": [],
      "collapsed_sections": [],
      "authorship_tag": "ABX9TyN3JAe8iGogfaLDJE/3qxiC",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
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
        "<a href=\"https://colab.research.google.com/github/VLNTNF/online-shoppers-dataset/blob/main/Online_Shoppers_Dataset.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "AX4UzWAcYB0e"
      },
      "source": [
        "import requests"
      ],
      "execution_count": 1,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "KS3fWCugYvMs",
        "outputId": "9686f042-40c7-4ba9-93d0-445330f7ab2f"
      },
      "source": [
        "url = \"https://www.afm-telethon.fr/telethon/bref/parrains-resultats-telethon-1379\"\r\n",
        "\r\n",
        "reponse = requests.get(url)\r\n",
        "reponse.status_code"
      ],
      "execution_count": 2,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "200"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 2
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "vOlfZpS4Y26n"
      },
      "source": [
        "html = reponse.content"
      ],
      "execution_count": 3,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZiLCzcJWY5jA"
      },
      "source": [
        "from bs4 import BeautifulSoup\r\n",
        "soup = BeautifulSoup(html, 'html.parser')\r\n",
        "element = soup.select_one('td')\r\n",
        "text_data = [element.get_text() for element in soup.select('td')]\r\n",
        "annee, parrain, montant = text_data[3::3], text_data[4::3], text_data[5::3]"
      ],
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "VdAv3Q2SZCPD",
        "outputId": "53c40f96-50fa-4d9d-eda0-155901299613"
      },
      "source": [
        "import pandas as pd\r\n",
        "df = pd.DataFrame({\"annee\":annee,\r\n",
        "                  \"parrain\":parrain,\r\n",
        "                  \"montant\":montant})\r\n",
        "df.head()"
      ],
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "execute_result",
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
              "      <th>annee</th>\n",
              "      <th>parrain</th>\n",
              "      <th>montant</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1987</td>\n",
              "      <td>Jerry LEWIS</td>\n",
              "      <td>29 650 000 €</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>1988</td>\n",
              "      <td>Mireille MATHIEU</td>\n",
              "      <td>28 490 000 €</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>1989</td>\n",
              "      <td>Alain DELON</td>\n",
              "      <td>40 930 000 €</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>1990</td>\n",
              "      <td>Claudia CARDINALE</td>\n",
              "      <td>46 510 000 €</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>1991</td>\n",
              "      <td>Jerry LEWIS, Mireille MATHIEU et Ornella MUTTI</td>\n",
              "      <td>38 650 000 €</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "  annee                                         parrain       montant\n",
              "0  1987                                     Jerry LEWIS  29 650 000 €\n",
              "1  1988                                Mireille MATHIEU  28 490 000 €\n",
              "2  1989                                     Alain DELON  40 930 000 €\n",
              "3  1990                               Claudia CARDINALE  46 510 000 €\n",
              "4  1991  Jerry LEWIS, Mireille MATHIEU et Ornella MUTTI  38 650 000 €"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 5
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "nJL2_MAWZH8Q",
        "outputId": "0a36a462-d94f-4286-b136-91f32fa785dd"
      },
      "source": [
        "df[\"annee\"] = df.annee.astype(int)\r\n",
        "df[\"montant\"] = df.montant.str.replace(\"\\D\", '').astype(int)\r\n",
        "df.dtypes"
      ],
      "execution_count": 6,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "annee       int64\n",
              "parrain    object\n",
              "montant     int64\n",
              "dtype: object"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 6
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 360
        },
        "id": "BqOjLye9ZL4G",
        "outputId": "3a3cea6e-19b9-4295-9a07-d6966ea09102"
      },
      "source": [
        "ax = df.plot(kind='bar', x=\"annee\", figsize=(15,5))"
      ],
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAA2oAAAFXCAYAAADTUL1RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4yLjIsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+WH4yJAAAgAElEQVR4nO3de5weZX338c8vBxIggJoEDwRIVJBTIEBELFoRUMEoaKsoBRW1UI/Y2scarY8H+lRj65GnWotKoz4cyqFCWqJQBKQqSML5JBgxQMBCCGcFIfp7/phJuFk32U12dvba2c/79drX3vfcs/Oda+69D7+5rpmJzESSJEmSVI5xI70CkiRJkqSnslCTJEmSpMJYqEmSJElSYSzUJEmSJKkwFmqSJEmSVBgLNUmSJEkqzIgWahFxUkTcExHXD2Le7SLiooi4KiKujYhXt7GOkiRJktS2ke5RWwgcPMh5Pwacnpl7Am8GvjpcKyVJkiRJI2lEC7XMvAS4r3daRDwvIr4fEVdExH9HxE5rZge2rG9vBdzV4qpKkiRJUmsmjPQK9ONE4F2Z+fOIeBFVz9kBwCeB8yPi/cDmwEEjt4qSJEmSNHyKKtQiYgrwR8AZEbFm8qT69xHAwsz8fES8GPhOROyWmb8fgVWVJEmSpGFTVKFGNRTzgcyc089j76Q+ni0zL42IycA04J4W10+SJEmSht1In0zkKTLzIeCXEfFGgKjsUT98O3BgPX1nYDKwckRWVJIkSZKGUWTmyIVHnArsT9UzdjfwCeBC4J+BZwMTgdMy8/iI2AX4OjCF6sQif5OZ54/EekuSJEnScBrRQk2SJEmS9IeKGvooSZIkSbJQkyRJkqTijNhZH6dNm5YzZ84cqXhJkiRJGlFXXHHFvZk5vb/HRqxQmzlzJkuXLh2peEmSJEkaURFx27oec+ijJEmSJBXGQk2SJEmSCmOhJkmSJEmFGbFj1CRJkiSNPk888QQrVqzgscceG+lVGTUmT57MjBkzmDhx4qD/xkJNkiRJ0qCtWLGCLbbYgpkzZxIRI706xctMVq1axYoVK5g1a9ag/86hj5IkSZIG7bHHHmPq1KkWaYMUEUydOnWDeyAt1CRJkiRtEIu0DbMx28tCTZIkSdKY98ADD/DVr351SMtYuHAhd911VyPr4zFqkiRJkjbazPnnNrq85QvmNbq8wVpTqL3nPe/Z6GUsXLiQ3Xbbjec85zlDXh971CRJkiSNKsuXL2ennXbi6KOPZscdd+TII4/kggsuYL/99mOHHXbg8ssv57777uN1r3sdu+++O/vuuy/XXnstAJ/85Cd5xzvewf77789zn/tcTjjhBADmz5/PL37xC+bMmcOHPvQhHnnkEQ488ED22msvZs+ezTnnnLM2e+edd+aYY45h11135ZWvfCWPPvooZ555JkuXLuXII49kzpw5PProo0Nqoz1qkiRprY3ZMz5Se78ljW3Lli3jjDPO4KSTTuKFL3whp5xyCj/60Y9YtGgRn/70p9l2223Zc889Ofvss7nwwgt561vfytVXXw3Az372My666CIefvhhXvCCF/Dud7+bBQsWcP3116+dZ/Xq1Xz3u99lyy235N5772Xffffl0EMPBeDnP/85p556Kl//+tc5/PDDOeusszjqqKP4p3/6Jz73uc8xd+7cIbfPQk2SJEnSqDNr1ixmz54NwK677sqBBx5IRDB79myWL1/ObbfdxllnnQXAAQccwKpVq3jooYcAmDdvHpMmTWLSpElsvfXW3H333X+w/Mzkox/9KJdccgnjxo3jzjvvXDvfrFmzmDNnDgB77703y5cvb7x9FmqSJEmSRp1JkyatvT1u3Li198eNG8fq1avXe3Hp3r8dP348q1ev/oN5Tj75ZFauXMkVV1zBxIkTmTlz5tpT7Pf9+6EOc+yPx6hJkiRJ6pyXvvSlnHzyyQBcfPHFTJs2jS233HKd82+xxRY8/PDDa+8/+OCDbL311kycOJGLLrqI2267bcDMvssYCnvUJEmSJHXOmpOG7L777my22WZ861vfWu/8U6dOZb/99mO33XbjkEMO4cMf/jCvfe1rmT17NnPnzmWnnXYaMPPoo4/mXe96F5tuuimXXnopm2666Uavf2TmRv/xUMydOzeXLl06ItmSJDVtQ0/CUeoJOLp0MpEutUUqyU033cTOO+880qsx6vS33SLiiszs98wjDn2UJEmSpMI49FGSpFHCHiJJGjvsUZMkSZKkwlioSZIkSdogI3Wei9FqY7aXhZokSZKkQZs8eTKrVq2yWBukzGTVqlVMnjx5g/7OY9QkSVLrunKWTGksmjFjBitWrGDlypUjvSqjxuTJk5kxY8YG/Y2FmiRJkqRBmzhxIrNmzRrp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\n",
            "text/plain": [
              "<Figure size 1080x360 with 1 Axes>"
            ]
          },
          "metadata": {
            "tags": [],
            "needs_background": "light"
          }
        }
      ]
    }
  ]
}
