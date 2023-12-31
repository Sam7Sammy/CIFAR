
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
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
      "cell_type": "code",
      "execution_count": 1,
      "metadata": {
        "id": "K4W4yDXCTRjG"
      },
      "outputs": [],
      "source": [
        "import numpy as np\n",
        "import tensorflow as tf\n",
        "import matplotlib.pyplot as plt\n",
        "from sklearn.linear_model import LogisticRegression\n",
        "from sklearn.metrics import accuracy_score\n",
        "from sklearn.preprocessing import StandardScaler"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# Load the CIFAR-10 dataset\n",
        "(X_train, y_train), (X_test, y_test) = tf.keras.datasets.cifar10.load_data()\n",
        "\n",
        "# Define the labels for \"airplane\" and \"bird\"\n",
        "airplane_label = 0\n",
        "bird_label = 2\n"
      ],
      "metadata": {
        "id": "GAZrVJLETay7"
      },
      "execution_count": 2,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Filter the dataset to only include \"airplane\" and \"bird\" images\n",
        "train_mask = np.isin(y_train, [airplane_label, bird_label])\n",
        "test_mask = np.isin(y_test, [airplane_label, bird_label])\n",
        "\n",
        "X_train_filtered = X_train[train_mask[:, 0]]\n",
        "y_train_filtered = y_train[train_mask[:, 0]]\n",
        "\n",
        "X_test_filtered = X_test[test_mask[:, 0]]\n",
        "y_test_filtered = y_test[test_mask[:, 0]]"
      ],
      "metadata": {
        "id": "2KeerDsQUEIY"
      },
      "execution_count": 3,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Reshape the input data to (num_samples, num_features)\n",
        "X_train_flat = X_train_filtered.reshape(X_train_filtered.shape[0], -1)\n",
        "X_test_flat = X_test_filtered.reshape(X_test_filtered.shape[0], -1)\n",
        "\n",
        "# Normalize pixel values to the range [0, 1]\n",
        "X_train_flat = X_train_flat.astype('float32') / 255.0\n",
        "X_test_flat = X_test_flat.astype('float32') / 255.0"
      ],
      "metadata": {
        "id": "EvMAMZUcUIMM"
      },
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "\n",
        "# Create a Logistic Regression model\n",
        "model = LogisticRegression(C=1.0, max_iter=10000, solver='saga', random_state=42)"
      ],
      "metadata": {
        "id": "MW56tt4BUODv"
      },
      "execution_count": 5,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Standardize features (optional but often recommended)\n",
        "scaler = StandardScaler()\n",
        "X_train_scaled = scaler.fit_transform(X_train_flat)\n",
        "X_test_scaled = scaler.transform(X_test_flat)"
      ],
      "metadata": {
        "id": "Mjz6Z4Y7UR1t"
      },
      "execution_count": 6,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# Train the model\n",
        "model.fit(X_train_scaled, y_train_filtered.ravel())\n",
        "\n",
        "# Make predictions on the test set\n",
        "y_pred = model.predict(X_test_scaled)\n",
        "\n",
        "# Calculate accuracy\n",
        "accuracy = accuracy_score(y_test_filtered, y_pred)\n",
        "\n",
        "print('Test accuracy:', accuracy)"
      ],
      "metadata": {
        "id": "ASH6njxMUWMw"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "Logistic Regression is used, and this model generates a accuracy of 76%. Inorder to achieve better accuracy CNN can be implemented, which falls under Deep Learning. This can be achieved my Machine Learning experts within the given time slot.\n",
        "\n",
        "\n"
      ],
      "metadata": {
        "id": "qDl0AZ5kXAAs"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# Initialize a list to store accuracy values\n",
        "accuracies = []\n",
        "\n",
        "# Train the model and track accuracy over iterations\n",
        "for i in range(1, 11):  # Reduced to 1,000 iterations\n",
        "    model.fit(X_train_scaled, y_train_filtered.ravel())\n",
        "    y_pred = model.predict(X_test_scaled)\n",
        "    accuracy = accuracy_score(y_test_filtered, y_pred)\n",
        "    accuracies.append(accuracy)\n",
        "\n",
        "# Create a plot of accuracy over iterations\n",
        "plt.plot(range(1, 11), accuracies)\n",
        "plt.xlabel('Iterations')\n",
        "plt.ylabel('Accuracy')\n",
        "plt.title('Accuracy Over Iterations (1,000 iterations)')\n",
        "plt.show()\n",
        "\n",
        "# Evaluate the model\n",
        "test_loss, test_accuracy = model.score(X_test_scaled, y_test_filtered)\n",
        "print(f'Test accuracy: {test_accuracy * 100:.2f}%')"
      ],
      "metadata": {
        "id": "WF8ugXJaVBI1"
      },
      "execution_count": null,
      "outputs": []
    }
  ]
}
