---
order: 0
title:
  zh-CN: 移动微信
  en-US: Type
---

## zh-CN

## en-US

````jsx
import React from 'react';
import { List, InputItem, Button } from 'antd-mobile';
import 'antd-mobile/dist/antd-mobile.css';

class Bind extends React.Component {

  constructor(props){
    super(props);
    this.state = {
      phone: '',
      name: '',
      check_code: '',
    };
  }

  handleSendCheckCode = () => {
    fetch('/v1/send_sms', {
      method: 'POST',
      credentials: 'include',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({phone: this.state.phone}),
    }).then(function(response) {
      return response.json();
    }).then(function(json) {
      if(!json.success) {
        alert(json.message);
      } else {
        alert('已经发送');
      }
    });
  }

  handleSubmit = () => {
    fetch('/wechat/my/do_bind', {
      method: 'PUT',
      credentials: 'include',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(this.state),
    }).then(function(response) {
      return response.json();
    }).then(function(json) {
      if(!json.success) {
        alert(json.message);
      } else {
        window.location = '/wechat/my/orders/';
      }
    });
  }

  render() {
    return (<div>
      <List renderHeader={() => '绑定用户'}>
        <InputItem clear placeholder="姓名" autoFocus onChange={(v) => this.setState({ name: v })} >姓名 </InputItem>
        <InputItem type="phone" clear placeholder="手机号码" onChange={(v) => this.setState({ phone: v })} >手机号码 </InputItem>
        <InputItem type="number" clear placeholder="验证码" onChange={(v) => this.setState({ check_code: v })} >验证码 </InputItem>

        <List.Item>
          <Button onClick={this.handleSendCheckCode} className="btn" type="default">点击获取验证码</Button>
        </List.Item>

        <Button onClick={this.handleSubmit} className="btn" type="primary">绑定</Button>
      </List>
    </div>
    );
  }
}

ReactDOM.render(
  <div>
    <Bind />
  </div>
, mountNode);
````
