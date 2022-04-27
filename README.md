# React-Components-Life-Cycle-Lesson
دورة حياة المكونات | Component Lifecycle

كل component يمر بعدد من المراحل ابتداء من وقت تكوينه أو تحديثة إلى حذفه أو تحديثه، وهي ثلاث مراحل رئيسية Mounting و Updating و Unmounting وكل مرحلة من هذهِ المراحل تحتوي على methods خاصة بها يتم استدعائها في تلك المرحلة بترتيب معين حيث أنها تمكنك من التحكم في تحديث state و UI بشكل أكبر، سنتعلم في هذا الدرس lifecycle الخاصة بمكون class component.



مرحلة Mounting

وهي المرحلة الأولى حيث يتم تكوين وصف UI الخاص بالمكون لإدخاله في DOM ومن أهم الدوال فيها هي `constructor` و `render` و `componentDidMount`.


دالّة constructor 
تستدعى هذهِ الدالة قبل عملية تكوين component أي قبل عملية إدخال component في DOM ومن أهم استخدامات هذهِ الدالة:

- تستخدم في تهيئة `state` حيث أنها المكان الوحيد الذي يتم إسناد قيمة مباشرة إلى `state` ولاتستخدم `setState` داخله.
- يتم ربط الدوال event handler بـالكائن.
    class Clicker extends React.Component {
      constructor(props) {
        super(props);
        this.handleClick = this.handleClick.bind(this);
        this.state = {
           clicks: 0
        };
      }
      handleClick() {
        this.setState({ 
          clicks: this.state.clicks + 1
        });
      }
      //...
    }


دالّة render
حيث أن هذهِ الدالة هي الدالة الوحيدة التي يجب كتابتها داخل class component ويتم وصف UI لـ component داخل هذه الدالة ويجب أن تكون هذهِ الدالة خالية من أي تعديل على component state.

    render() {
        return (
          <div className="Clicker">
            <h4>{this.state.clicks}</h4>
            <button onClick={this.handleClick}>Add One</button>
          </div>
        )
    }


دالّة componentDidMount
يتم استدعاء هذهِ الدالة بعد أن يتم تكوين component وإدخاله داخل DOM حيث أن هذهِ الدالة مكان جيد لعمل طلب بيانات من موقع معين (initiate AJAX request) أو إضافة event listeners لـ component وتنفيذ الإجراء المطلوب في حالة حدوثه.

    componetDidMount() {
      fetch('https://gitconnected.com')
        .then((res) => {
          this.setState({
            user: res.user
          });
        });
    }



مرحلة Updating

ننشأ هذهِ المرحلة في حالة تحديث props أو state حيث يتم إعادة تكوين component ومن أهم الدوال في هذهِ المرحلة هي `render` و `componentDidUpdate`.

دالّة render
هذهِ الدالة هي الدالة الوحيدة التي يجب كتابتها داخل class component ويتم وصف UI لـ component داخل هذهِ الدالة ويجب أن تكون خالية من أي تعديل على component state.

دالّة componentDidUpdate
تستدعى هذهِ الدالة مباشرة بعد أن يتم إعادة تكوين component وفي حالة استدعاء دالة `setState` داخل هذهِ الدالة يجب أن تكون محاطة بشرط معين وإلا ستحدث عملية updating إلى مالا نهاية.

مرحلة Unmounting

تنشأ هذهِ المرحلة عندما يتم البدء في حذف component من DOM وتحتوي على دالة واحدة فقط وهي `componentWillUnmount`.


دالّة componentWillUnmount
تستدعى هذهِ الدالة مباشرة قبل أن يتم حذف component من DOM حيث تعتبر مكان جيد لإزالة الأشياء المتعلقة بـ component مثل حذف event listeners تمت إضافته مسبقا لـ component.


    handleResize() {
     this.setState({
        windowWidth: window.innerWidth});
    }
    componentDidMount(){
      window.addEventListener('resize', this.handleResize);
    }
    componentWillUnmount() {
      window.removeEventListener('resize', this.handleResize);
    }
