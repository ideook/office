---
title: 16강 Material UI 모달(Modal) 디자인 구현하기
date: 2022-04-21
author: ideook
layout: post
---

이번 시간에는 모달(Modal) 기능을 이용해서 고객 추가(Customer Add) 기능을 모달 창에서 띄우는 방법에 대해서 알아보도록 하겠습니다. 따라서 Material UI의 Dialog를 import하여 디자인 요소를 활용해야 합니다. 우리는 모달 중에서 다이얼로그(Dialog)를 활용할 것이며 이를 위해서 Dialog 컴포넌트를 사용해야 합니다.

▶ CustomerAdd.tsx

```tsx
import React, { useState } from "react";
import axios from "axios";
import Dialog from "@mui/material/Dialog";
import DialogActions from "@mui/material/DialogActions";
import DialogContent from "@mui/material/DialogContent";
import DialogTitle from "@mui/material/DialogTitle";
import TextField from "@mui/material/TextField";
import Button from "@mui/material/Button";
import { createTheme } from "@mui/material/styles";
import { makeStyles } from "@mui/styles";

interface ICustomerAdd {
  stateRefresh: () => void;
}

const CustomerAdd: React.FunctionComponent<ICustomerAdd> = props => {
  //const [file, setFile] = React.useState<File | null>(null);
  const [userName, setUserName] = useState<string>("");
  const [birthday, setBirthday] = useState<string>("");
  const [gender, setGender] = useState<string>("");
  const [job, setJob] = useState<string>("");
  const [fileName, setFileName] = useState<string>("");
  const [open, setOpen] = useState<boolean>(false);

  const handleFormSubmit = (e: React.MouseEvent<HTMLElement>, text: string) => {
    e.preventDefault();
    addCustomer().then((response) => {
      console.log(response.data);
      props.stateRefresh();
    });

    setUserName("");
    setBirthday("");
    setGender("");
    setJob("");
  };

  //   const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  //     setFile(e.target.files ? e.target.files[0] : null);
  //     setFileName(e.target.value);
  //   };

  const addCustomer = () => {
    const url = "/api/customers";
    const formData = new FormData();
    //formData.append("image", file);
    formData.append("name", userName);
    formData.append("birthday", birthday);
    formData.append("gender", gender);
    formData.append("job", job);
    const config = {
      headers: {
        "content-type": "multipart/form-data",
      },
    };
    return axios.post(url, formData, config);
  };

  const handleClickOpen=()=> {
    setOpen(true);
  }

  const handleClose=()=> {
    setUserName("");
    setBirthday("");
    setGender("");
    setJob("");
    setOpen(false);
  }

  return (
    <div>
    <Button variant="contained" color="primary" onClick={handleClickOpen}>
      고객 추가하기
    </Button>
    <Dialog open={open} onClose={handleClose}>
      <DialogTitle>고객 추가</DialogTitle>
      <DialogContent>
        {/* <input className={classes.hidden} accept="image/*" id="raised-button-file" type="file" file={this.state.file} value={this.state.fileName} onChange={this.handleFileChange} />
        <label htmlFor="raised-button-file">
          <Button variant="contained" color="primary" component="span" name="file">
            {this.state.fileName === ''? "프로필 이미지 선택" : fileName}
          </Button>
        </label><br/> */}
        <TextField label="이름" type="text" name="userName" value={userName} onChange={(e)=>{ 
           setUserName(e.target.value);
        }} /><br/>
        <TextField label="생년월일" type="text" name="birthday" value={birthday} onChange={(e)=>{ 
           setBirthday(e.target.value);
        }} /><br/>
        <TextField label="성별" type="text" name="gender" value={gender} onChange={(e)=>{ 
           setGender(e.target.value);
        }} /><br/>
        <TextField label="직업" type="text" name="job" value={job} onChange={(e)=>{ 
           setJob(e.target.value);
        }} /><br/>
      </DialogContent>
      <DialogActions>
        <Button variant="contained" color="primary" onClick={(e) => handleFormSubmit(e, "clicked")}>추가</Button>
        <Button variant="outlined" color="primary" onClick={handleClose}>닫기</Button>
      </DialogActions>
    </Dialog>
  </div>
  );
}

export default CustomerAdd;
```

▶ CustomerDelete.tsx

```tsx
import React from 'react';
import Button from "@mui/material/Button";
import Dialog from "@mui/material/Dialog";
import DialogTitle from "@mui/material/DialogTitle";
import DialogContent from "@mui/material/DialogContent";
import DialogActions from "@mui/material/DialogActions";
import Typography from "@mui/material/Typography";

class CustomerDelete extends React.Component {

    constructor(props) {
        super(props);
        this.state = {
          open: false
        }
        this.handleClickOpen = this.handleClickOpen.bind(this)
        this.handleClose = this.handleClose.bind(this);
    }

    handleClickOpen() {
        this.setState({
          open: true
        });
    }

    handleClose() {
        this.setState({
          open: false
        })
    }

    deleteCustomer(id){
        const url = '/api/customers/delete/' + id;
        fetch(url, {
           method: 'DELETE'
        });
        this.props.stateRefresh();
    }

    render() {
        return (
            <div>
                <Button variant="contained" color="secondary" onClick={this.handleClickOpen}>
                    삭제
                </Button>
                <Dialog onClose={this.handleClose} open={this.state.open}>
                    <DialogTitle onClose={this.handleClose}>
                        삭제 경고
                    </DialogTitle>
                    <DialogContent>
                        <Typography gutterBottom>
                            선택한 고객 정보가 삭제됩니다.
                        </Typography>
                    </DialogContent>
                    <DialogActions>
                        <Button variant="contained" color="primary" onClick={(e) => {this.deleteCustomer(this.props.id)}}>삭제</Button>
                        <Button variant="outlined" color="primary" onClick={this.handleClose}>닫기</Button>
                    </DialogActions>
                </Dialog>
            </div>
        )
    }
}

export default CustomerDelete;
```

※ 실행결과 ※

![](images/2022-04-21-11-49-43.png)

고객을 추가합시다.

![](images/2022-04-21-11-49-46.png)

이어서 삭제도 해봅시다.

![](images/2022-04-21-11-49-49.png)

삭제도 잘 되네요.

![](images/2022-04-21-11-49-53.png)

출처: https://ndb796.tistory.com/231?category=1030599 [안경잡이개발자]
