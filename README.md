from kivy.app import App
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.uix.boxlayout import BoxLayout
from kivy.lang import Builder

# AdMob के लिए लेआउट
Builder.load_string('''
<AdBanner>:
    size_hint: (1, None)
    height: '50dp'
''')

class AdBanner(BoxLayout):
    pass

class CashQuizGame(App):
    def build(self):
        self.score = 0
        self.questions = [
            {"question": "भारत की राजधानी क्या है?", "answer": "दिल्ली"},
            {"question": "सूर्य की परिक्रमा करने वाला पहला ग्रह कौन सा है?", "answer": "बुध"},
            {"question": "Python किस प्रकार की भाषा है?", "answer": "प्रोग्रामिंग"}
        ]
        self.current_question = 0

        # मुख्य लेआउट
        layout = BoxLayout(orientation='vertical')

        # AdMob बैनर (अपना Ad Unit ID डालें)
        self.admob_banner = AdBanner()
        layout.add_widget(self.admob_banner)

        # प्रश्न और उत्तर बटन
        self.question_label = Label(text=self.questions[self.current_question]["question"])
        self.answer_btn = Button(text="जवाब देखें", on_press=self.check_answer)
        
        layout.add_widget(self.question_label)
        layout.add_widget(self.answer_btn)
        return layout
    
    def check_answer(self, instance):
        correct_answer = self.questions[self.current_question]["answer"]
        self.question_label.text = f"सही जवाब: {correct_answer}\nअगला प्रश्न..."
        self.score += 10
        
        # इंटरस्टिशियल एड दिखाएं (हर दूसरे जवाब पर)
        if self.current_question % 2 == 0:
            self.show_interstitial_ad()

        # अगला प्रश्न लोड करें
        self.current_question += 1
        if self.current_question < len(self.questions):
            self.question_label.text = self.questions[self.current_question]["question"]
        else:
            self.question_label.text = f"गेम खत्म! आपका स्कोर: ₹{self.score}"
            self.answer_btn.disabled = True

    def show_interstitial_ad(self):
        # अपना Ad Unit ID डालें (उदाहरण: AdMob टेस्ट ID)
        interstitial_ad_id = "ca-app-pub-3940256099942544/1033173712"
        print(f"[AdMob] Interstitial Ad Loaded: {interstitial_ad_id}")

if __name__ == '__main__':
    CashQuizGame().run()
