docker --version       #To check the docker version

docker info                 #for display indo of docker

mkdir AI-Docker-Project        #to  create folder

cd AI-Docker-Project      #open that folder


#### to make the in folder#####

New-Item app.py -ItemType File


New-Item requirements.txt -ItemType File


New-Item Dockerfile -ItemType File



#TO open the app.py in notepad
notepad app.py         

##########################################################################################
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/")
def home():
    return """
    <h1>AI Model Deployment Using Docker</h1>
    <p>Application is running successfully!</p>
    <p>Use the /predict endpoint to test the model.</p>
    """

@app.route("/predict", methods=["GET", "POST"])
def predict():
    if request.method == "POST":
        data = request.get_json(silent=True) or {}
        value = data.get("value", 0)
    else:
        value = request.args.get("value", default=0, type=float)

    try:
        value = float(value)
    except (ValueError, TypeError):
        return jsonify({"error": "Please provide a valid number."}), 400

    # Simple example prediction rule
    if value >= 50:
        prediction = "High"
    else:
        prediction = "Low"

    return jsonify({
        "input": value,
        "prediction": prediction
    })

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
    
################################################################################################################


notepad requirements.txt    # TO open req file in notepad

########
Flask
########


notepad Dockerfile              #to open dockerfile in notepad

####################################################################################

FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]

########################################################################



docker build -t ai-model-app .          # To build a docker image


docker images             #Look for ai-model-app in the output.



#Run the Docker container
docker run -d -p 5000:5000 --name ai-container ai-model-app




docker ps       #to verify docker is running



#To inspect its logs
docker logs ai-container   


##Test the application in your browser##
Open your browser and visit:
--------------------------------------------------
http://localhost:5000
You should see:
AI Model Deployment Using Docker
Application is running successfully!
-------------------------------------------


Test the prediction endpoint
Open this address in your browser:
http://localhost:5000/predict?value=75
Expected output:
{
  "input": 75.0,
  "prediction": "High"
}
---------------------------------------------------

Now test a lower value:
http://localhost:5000/predict?value=25
Expected output:
{
  "input": 25.0,
  "prediction": "Low"
}
-------------------------------------------



# To stop the ai container
docker stop ai-container


#To verify
docker ps
