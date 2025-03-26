from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.core.audio import SoundLoader
from random import choice

# Define responses with emotions
responses = {
    "hello": ["Hi there! 😊", "Hey! How are you? 😃", "Hello, my dear! 💖"],
    "how are you": ["I'm great! What about you? 😊", "Feeling happy today! 😍"],
    "i love you": ["Aww, I love you too! 💕", "You're so sweet! 😘"],
    "bye": ["Goodbye! Talk soon! 👋", "Miss you already! 😢"],
    "default": ["I'm not sure what you mean. 🤔"]
}

class UrsulaChat(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(orientation='vertical', **kwargs)
        
        self.chat_display = Label(text="Ursula: Hello! 💖", size_hint_y=0.8)
        self.add_widget(self.chat_display)
        
        self.user_input = TextInput(hint_text="Type a message...", multiline=False, size_hint_y=0.1)
        self.add_widget(self.user_input)
        
        self.send_button = Button(text="Send", size_hint_y=0.1)
        self.send_button.bind(on_press=self.send_message)
        self.add_widget(self.send_button)
    
    def send_message(self, instance):
        user_text = self.user_input.text.lower()
        response = self.get_response(user_text)
        
        self.chat_display.text += f"\nYou: {self.user_input.text}"
        self.chat_display.text += f"\nUrsula: {response}"
        
        self.play_voice()
        
        self.user_input.text = ""
    
    def get_response(self, message):
        for key in responses:
            if key in message:
                return choice(responses[key])
        return choice(responses["default"])
    
    def play_voice(self):
        sound = SoundLoader.load("voice.mp3")  # Ensure you have a voice.mp3 file in the folder
        if sound:
            sound.play()

class UrsulaApp(App):
    def build(self):
        return UrsulaChat()

if __name__ == "__main__":
    UrsulaApp().run()

