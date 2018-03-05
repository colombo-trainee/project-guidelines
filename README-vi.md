
[中文版](./README-zh.md)
 | [日本語版](./README-ja.md)
 | [한국어](./README-ko.md)

[<img src="./images/elsewhen-logo.png" width="180" height="180">](http://elsewhen.co/)


# Project Guidelines &middot; [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
> While developing a new project is like rolling on a green field for you, maintaining it is a potential dark twisted nightmare for someone else.
Here's a list of guidelines we've found, written and gathered that (we think) works really well with most JavaScript projects here at [elsewhen](http://elsewhen.co).
If you want to share a best practice, or think one of these guidelines should be removed, [feel free to share it with us](http://makeapullrequest.com).
- [Git](#git)
    - [Some Git rules](#some-git-rules)
    - [Git workflow](#git-workflow)
    - [Writing good commit messages](#writing-good-commit-messages)
- [Documentation](#documentation)
- [Environments](#environments)
    - [Consistent dev environments](#consistent-dev-environments)
    - [Consistent dependencies](#consistent-dependencies)
- [Dependencies](#dependencies)
- [Testing](#testing)
- [Structure and Naming](#structure-and-naming)
- [Code style](#code-style)
    - [Some code style guidelines](#code-style-check)
    - [Enforcing code style standards](#enforcing-code-style-standards)
- [Logging](#logging)
- [API](#api)
    - [API design](#api-design)
    - [API security](#api-security)
    - [API documentation](#api-documentation)
- [Licensing](#licensing)

<a name="git"></a>
## 1. Git
![Git](/images/branching.png)
<a name="some-git-rules"></a>

### 1.1 Some Git rules
Có một bộ quy tắc cần lưu ý:
* Thực hiện công việc tại một nhánh feature.
    
    _Tại sao:_
    >Bởi vì cách này toàn bộ công việc được hoàn thành cô lập trong một nhánh riêng hơn là nhánh chính. Nó cho phép bạn gửi nhiều pull request mà không có sự nhầm lẫn. Bạn có thể lặp lại mà không làm bẩn nhành chính với code chưa hoàn thiện có khả năng không ổn định [đọc thêm...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
* Nhánh ra từ `develop`
    
    _Tại sao:_
    >Cách này, bạn có thể chắc chắn rằng code ở master sẽ hầu như luôn xây dựng mà không vấn đề gì, và hầu hết có thể dùng luôn cho bản phát hành(điều này có thể quá mức cần thiết với một số dự án).

* Không bao giờ đẩu lên nhánh `develop` hoặc `master` . Hãy tạo một Pull Request.
    
    _Why:_
    > Nó thông báo cho các thành biên rằng họ đã hoàn thiện một tính năng. Nó cũng cho phép dễ dàng peer-review code và dành ra diễn đàn để thảo luận về tính năng được đề xuất.

* Cập nhật nhánh `develop` địa phương và thực hiện tương tác rebase trước khi đẩy tính năng của và tạo một Pull Request.

    _Why:_
    > Việc tái cơ cấu sẽ hợp nhất với nhánh yêu cầu (`master` hoặc `develop`) và áp dụng những commit mà bạn đã thực hiện ở phần đầu của lịch sử mà không tạo ra một merge commit(giả sử không có xung đột)[đọc thêm ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Giải quyết xung đột ngầm trong khi đang tái cơ cáu và trước khi tạo Pull Request.
* Xóa các chi nhánh tính năng cục bộ sau khi merging.
    
    _Tại sao:_
    > Nó sẽ hỗn tạp lên danh sách các nhánh của bạn với các nhánh chết. Nó đản bản bạo chỉ mãi merge nhánh trở lại (`master` hoặc `develop`) một lần. Nhánh tính năng chỉ nên tồn tại trong khi công việc vẫn đang được tiến hành.

* Trước khi tạo một Pull Request, hãy chắc nhánh tính năng xây dựng thành công và vượt qua kiểm tra (bao gồm cả kiểm tra phong cách code).
    
    _Tại sao:_
    > Bạn sắp thêm code của bạn vào một nhánh ổn định. Nếu nhánh tính năng của bạn kiểm tra lỗi, có cơ hội cao rằng việc xây dựng nhánh đích sẽ cũng thất bại. Ngoài ra, bạn cần áp dụng kiểm tra phong cách code trước khi tạo ra một Pull Request. Nó giúp dễ đọc và giảm cơ hội định dạng các bản sửa lỗi được trộn lẫn vào những thay đổi thực tế.  

* Sử dụng [tệp này](./.gitignore) `.gitignore` .
    
    _Tại sao:_
    > Đã có một danh sách các file không nên gửi vứi doe của bạn đến remove repository. Ngoài ra, nó loại trừ các thư mục và các file cài đặt cho hầu hết biên tập viên được sử dụng, cũng như các thư mục phụ thuộc phổ biến.

* Bảo vệ nhánh `develop` và `master`.
  
    _Tại sao:_
    > Nó bỏa vệ những nhánh production-ready được thay đổi bất ngờ và không thể thay đổi. đọc thêm... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) và [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)

<a name="git-workflow"></a>
### 1.2 Quy trình làm việc của Git
Vì hầu hết các lí do trên, chúng ta sử dụng [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) với [Interactive Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) và vài phẩn tử [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (đặt tên và có một nhánh develop). Các bước chính như sau:

* Cho một dự án mới, khởi tạo một repo git trong thư mục dự án. __Đối với tính năng/thay đổi bước này nên được bỏ qua__.
   ```sh
   cd <project directory>
   git init
   ```

* Vào  một nhánh tính năng (bug-fix) .
    ```sh
    git checkout -b <branchname>
    ```
* Thay đổi.
    ```sh
    git add
    git commit -a
    ```
    _Why:_
    > `git commit -a` sẽ bắt đầu trình biên tập cho phép bạn tách riêng chủ thể ra khỏi body. Đọc thêm tại *section 1.3*.

* Đồng bộ hóa từ xa để có được những thay đổi bạn đã bỏ qua.
    ```sh
    git checkout develop
    git pull
    ```
    
    _Tại sao:_
    > Nó sẽ mang cho bạn một cơ hội để đối phó với các xung đột trên máy của bạn khi rebasing(say) thay vì tạo ra một Pull Request có chứa xung đột.
    
* Cập nhật nhánh tính năng của bạn với những thay đổi mới nhất từ develop bởi tương tác rebase.
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```
    
    _Tại sao:_
    > Bạn có thể sử dụng --autosquash để giấu tất cả commit đến một commit duy nhất. Không một ai muốn nhiều commit cho một nhánh tính năng trên nhánh develop. [read more...](https://robots.thoughtbot.com/autosquashing-git-commits)
    
* Nếu bạn không có xung đột, bỏ qua bước này. Nếu bnaj có xung đột, [giải quyết nó](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)  và tiếp tục rebase.
    ```sh
    git add <file1> <file2> ...
    git rebase --continue
    ```
* Đẩy nhánh của bạn. Rebase sẽ thay đổi lịch sử, vì vậy bank sẽ phải sử dụng `-f` để bược các thay đổi vào các nhánh từ xa. Nếu ai đó đang làm việc trên nhánh của bạn, sử dụng phương pháp gây ít tổn hại hơn `--force-with-lease`.
    ```sh
    git push -f
    ```
    
    _Tại sao:_
    > Khi bạn làm một rebase, bạn đang thay đổi lịch sử trên một nhánh tính năng. Kết quả là, Git sẽ từ chối `git push` bình thường. Thay vào đó, bạn cần sử dụng cờ -f hoặc --force. [đọc thêm...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
    
    
* Tạo một Pull Request .
* Pull Request sẽ được chấp nhận,sáp nhập và đóng bởi người đánh giá.
* Xóa nhánh tính năng địa phương nếu bạn hoàn thành.

  ```sh
  git branch -d <branchname>
  ```
  để xóa toàn bộ nhánh không còn ở xa
  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>
### 1.3 Viết tin nhắn commit tốt

Có một hướng dẫn tốt cho việc tạo commit gắn bó với nó làm cho việc làm việc với Git và cộng tác với người khác dễ dàng hơn. Ở đây chúng ta có một vài luật của thumb ([Nguồn](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * Tách  subject từ body với một dòng mới giữa hai.

    _Tại sao:_
    > Gits đủ thông minh để phân biệt dòng đầu tiên của thông báo commit như dạng tóm lược của bạn. Sự thật, nếu bạn dùng thử git shortlog, thay thế cho git log, bạn sẽ nhìn thấy một dãy dài thông báo commit, bao gồm cả id của commit, và bản tóm lược.

 * Giới hạn dòng chủ đề 50 kí tự và bao bọc cwo thế ở 72 kí tự.

    _tại sao_
    > Các commit nên được làm tốt và tập trung nhất ccos thể, nó không phải là nơi để rườm rà . [read more...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

 * Viết in dòng chủ đề.
 * Không kết thúc dòng chủ đề với một giai đoạn  .
 * Sử dụng [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) trong dòng chủ đề.

    _Tại sao:_
    > Thay vì viết một commit nói rằng committer vừa làm xong. Tốt nhất xem các thông báo này như hướng dẫn cho những gì sẽ thực hiện sau khi commit được áp dụng trên repo. [read more...](https://news.ycombinator.com/item?id=2079612)


 * Dùng phần thân để giải thích **what** và **why** như trái ngược với **how**.

 <a name="documentation"></a>
## 2. Documentation

![Documentation](/images/documentation.png)

* Use this [template](./README.sample.md) for `README.md`, Feel free to add uncovered sections.
* For projects with more than one repository, provide links to them in their respective `README.md` files.
* Keep `README.md` updated as a project evolves.
* Comment your code. Try to make it as clear as possible what you are intending with each major section.
* If there is an open discussion on github or stackoverflow about the code or approach you're using, include the link in your comment. 
* Don't use comments as an excuse for a bad code. Keep your code clean.
* Don't use clean code as an excuse to not comment at all.
* Keep comments relevant as your code evolves.

<a name="environments"></a>
## 3. Environments

![Environments](/images/laptop.png)

* Define separate `development`, `test` and `production` environments if needed.

    _Why:_
    > Different data, tokens, APIs, ports etc... might be needed in different environments. You may want an isolated `development` mode that calls fake API which returns predictable data, making both automated and manual testing much easier. Or you may want to enable Google Analytics only on `production` and so on. [read more...](https://stackoverflow.com/questions/8332333/node-js-setting-up-environment-specific-configs-to-be-used-with-everyauth)


* Load your deployment specific configurations from environment variables and never add them to the codebase as constants, [look at this sample](./config.sample.js).

    _Why:_
    > You have tokens, passwords and other valuable information in there. Your config should be correctly separated from the app internals as if the codebase could be made public at any moment.

    _How:_
    >
    `.env` files to store your variables and add them to `.gitignore` to be excluded. Instead, commit a `.env.example`  which serves as a guide for developers. For production, you should still set your environment variables in the standard way.
    [read more](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

* It’s recommended to validate environment variables before your app starts.  [Look at this sample](./configWithTest.sample.js) using `joi` to validate provided values.
    
    _Why:_
    > It may save others from hours of troubleshooting.

<a name="consistent-dev-environments"></a>
### 3.1 Consistent dev environments:
* Set your node version in `engines` in `package.json`.
    
    _Why:_
    > It lets others know the version of node the project works on. [read more...](https://docs.npmjs.com/files/package.json#engines)

* Additionally, use `nvm` and create a  `.nvmrc`  in your project root. Don't forget to mention it in the documentation.

    _Why:_
    > Any one who uses `nvm` can simply use `nvm use` to switch to the suitable node version. [read more...](https://github.com/creationix/nvm)

* It's a good idea to setup a `preinstall` script that checks node and npm versions.

    _Why:_
    > Some dependencies may fail when installed by newer versions of npm.
    
* Use Docker image if you can.

    _Why:_
    > It can give you a consistent environment across the entire workflow. Without much need to fiddle with dependencies or configs. [read more...](https://hackernoon.com/how-to-dockerize-a-node-js-application-4fbab45a0c19)

* Use local modules instead of using globally installed modules.

    _Why:_
    > Lets you share your tooling with your colleague instead of expecting them to have it globally on their systems.


<a name="consistent-dependencies"></a>
### 3.2 Consistent dependencies:

* Make sure your team members get the exact same dependencies as you.

    _Why:_
    > Because you want the code to behave as expected and identical in any development machine [read more...](https://medium.com/@kentcdodds/why-semver-ranges-are-literally-the-worst-817cdcb09277)

    _how:_
    > Use `package-lock.json` on `npm@5` or higher

    _I don't have npm@5:_
    > Alternatively you can use `Yarn` and make sure to mention it in `README.md`. Your lock file and `package.json` should have the same versions after each dependency update. [read more...](https://yarnpkg.com/en/)

    _I don't like the name `Yarn`:_
    > Too bad. For older versions of `npm`, use `—save --save-exact` when installing a new dependency and create `npm-shrinkwrap.json` before publishing. [read more...](https://docs.npmjs.com/files/package-locks)

<a name="dependencies"></a>
## 4. Dependencies

![Github](/images/modules.png)

* Keep track of your currently available packages: e.g., `npm ls --depth=0`. [read more...](https://docs.npmjs.com/cli/ls)
* See if any of your packages have become unused or irrelevant: `depcheck`. [read more...](https://www.npmjs.com/package/depcheck)
    
    _Why:_
    > You may include an unused library in your code and increase the production bundle size. Find unused dependencies and get rid of them.

* Before using a dependency, check its download statistics to see if it is heavily used by the community: `npm-stat`. [read more...](https://npm-stat.com/)
    
    _Why:_
    > More usage mostly means more contributors, which usually means better maintenance, and all of these result in quickly discovered bugs and quickly developed fixes.

* Before using a dependency, check to see if it has a good, mature version release frequency with a large number of maintainers: e.g., `npm view async`. [read more...](https://docs.npmjs.com/cli/view)

    _Why:_
    > Having loads of contributors won't be as effective if maintainers don't merge fixes and patches quickly enough.

* If a less known dependency is needed, discuss it with the team before using it.
* Always make sure your app works with the latest version of its dependencies without breaking: `npm outdated`. [read more...](https://docs.npmjs.com/cli/outdated)

    _Why:_
    > Dependency updates sometimes contain breaking changes. Always check their release notes when updates show up. Update your dependencies one by one, that makes troubleshooting easier if anything goes wrong. Use a cool tool such as [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

* Check to see if the package has known security vulnerabilities with, e.g., [Snyk](https://snyk.io/test?utm_source=risingstack_blog).


<a name="testing"></a>
## 5. Testing
![Testing](/images/testing.png)
* Có một môi trường chế độ `test` nếu cần.

    _Tại sao:_
    >  Trong khi thinh thoảng end to end testing trong chế độ `production` có thể xem là đủ, thì cũng có những ngoại lệ: Một ví dụ như bạn không muốn cho phép phân tích thông tin trong một chế độ 'production' và làm xấu bảng điều khiển với của ai đó với dữ liệu test. Một ví dụ khác là API của bạn có thể đạt đến giới hạn trong `production` và khóa lời gọi test sau một số lượng request xác định.

* Đặt các file test của bạn đến kế tiếp modules đã test sử dụng quy ước đặt tên `*.test.js` hoặc `.spec.js` giống như `moduleName.spec.js` .

    _Tại sao:_
    > Bạn không muốn đào xuyên cấu trúc thư mục để tìm một unit test. [đọc thêm...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)
    

* Đặt các file test bổ sung vào một thư mục riêng để tránh nhầm lẫn.

    _Tại sao:_
    > Một số file test không đặc biệt liên quan đến file cụ thể nào. Bạn phải đặt nó vào một file mà rất có thể được tìm bởi các nhà phát triển khác: thư mục `__test__` cũng là chuẩn bây giờ và được chọn bởi hầu hết testing framework của JavaScript

* Viết code có thể test được, tránh ảnh hưởng phụ, ảnh hưởng mở rộng, viết các hàm thuần túy 

    _Tại sao:_
    > Bạn muốn test một luồng làm việc logic như một đơn vị riêng biệt. Bạn phải "giảm thiểu tác động ngẫu nhiên và các quy trình không xác thực đối với độ tin cậy code của bạn". [đọc thêm...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)
    
    > Một hàm thuần túy là một hàm luôn trả về đầu ra giống nhau khi đầu vào giống nhau. Ngược lại một hàm bẩn là hàm có thể có các ảnh hưởng phụ hoặc phụ thuộc vào điều kiện từ bên ngoài để tạo ra giá trị. Điều này làm nó ít dự đoán trước được [đọc thêm...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

* Sử dụng một bộ kiểm tra kiểu tĩnh

    _Tại sao:_
    > Thỉnh thoảng bạn có thể cần một bộ kiểm tra kiểu tĩnh. Nó mang cho  bạn một mức độ tin cậy nhất định cho mã của bạn  [đọc thêm...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)


* Chạy test cục bộ trước khi thực hiện bất kì pull request nào đến `develop`.

    _Tại sao:_
    > Bạn không muốn là người gây ra việc xây dựng nhanh production-ready thất bại. Chạy những test của bạn sau khi `rebase` và trước khi đẩy feature-branch của bạn đến  một remote repository.

* Tài liệu các test của bạn bao gồm các hướng dẫn trong phần có liên quan đến file `README.md` của bạn.

    _Tại sao:_
    > Đây là một lưu ý tiện dụng bạn để lịa cho các nhà phát triển khác hoặc các chuyên gia DevOps hoặc QA hoặc bất kỳ ai có đủ may mắn để làm việc với code của bạn.

<a name="structure-and-naming"></a>
## 6. Structure and Naming
![Structure and Naming](/images/folder-tree.png)
* Organize your files around product features / pages / components, not roles. Also, place your test files next to their implementation.


    **Bad**

    ```
    .
    ├── controllers
    |   ├── product.js
    |   └── user.js
    ├── models
    |   ├── product.js
    |   └── user.js
    ```

    **Good**

    ```
    .
    ├── product
    |   ├── index.js
    |   ├── product.js
    |   └── product.test.js
    ├── user
    |   ├── index.js
    |   ├── user.js
    |   └── user.test.js
    ```

    _Why:_
    > Instead of a long list of files, you will create small modules that encapsulate one responsibility including its test and so on. It gets much easier to navigate through and things can be found at a glance.

* Put your additional test files to a separate test folder to avoid confusion.

    _Why:_
    > It is a time saver for other developers or DevOps experts in your team.

* Use a `./config` folder and don't make different config files for different environments.

    _Why:_
    >When you break down a config file for different purposes (database, API and so on); putting them in a folder with a very recognizable name such as `config` makes sense. Just remember not to make different config files for different environments. It doesn't scale cleanly, as more deploys of the app are created, new environment names are necessary.
    Values to be used in config files should be provided by environment variables. [read more...](https://medium.com/@fedorHK/no-config-b3f1171eecd5)
    

* Put your scripts in a `./scripts` folder. This includes `bash` and `node` scripts.

    _Why:_
    >It's very likely you may end up with more than one script, production build, development build, database feeders, database synchronization and so on.
    

* Place your build output in a `./build` folder. Add `build/` to `.gitignore`.

    _Why:_
    >Name it what you like, `dist` is also cool. But make sure that keep it consistent with your team. What gets in there is most likely generated  (bundled, compiled, transpiled) or moved there. What you can generate, your teammates should be able to generate too, so there is no point committing them into your remote repository. Unless you specifically want to. 

<a name="code-style"></a>
## 7. Code style

![Code style](/images/code-style.png)

<a name="code-style-check"></a>
### 7.1 Some code style guidelines

* Use stage-2 and higher JavaScript (modern) syntax for new projects. For old project stay consistent with existing syntax unless you intend to modernise the project.

    _Why:_
    > This is all up to you. We use transpilers to use advantages of new syntax. stage-2 is more likely to eventually become part of the spec with only minor revisions. 

* Include code style check in your build process.

    _Why:_
    > Breaking your build is one way of enforcing code style to your code. It prevents you from taking it less seriously. Do it for both client and server-side code. [read more...](https://www.robinwieruch.de/react-eslint-webpack-babel/)

* Use [ESLint - Pluggable JavaScript linter](http://eslint.org/) to enforce code style.

    _Why:_
    > We simply prefer `eslint`, you don't have to. It has more rules supported, the ability to configure the rules, and ability to add custom rules.

* We use [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for JavaScript, [Read more](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Use the javascript style guide required by the project or your team.

* We use [Flow type style check rules for ESLint](https://github.com/gajus/eslint-plugin-flowtype) when using [FlowType](https://flow.org/).

    _Why:_
    > Flow introduces few syntaxes that also need to follow certain code style and be checked.

* Use `.eslintignore` to exclude files or folders from code style checks.

    _Why:_
    > You don't have to pollute your code with `eslint-disable` comments whenever you need to exclude a couple of files from style checking.

* Remove any of your `eslint` disable comments before making a Pull Request.

    _Why:_
    > It's normal to disable style check while working on a code block to focus more on the logic. Just remember to remove those `eslint-disable` comments and follow the rules.

* Depending on the size of the task use  `//TODO:` comments or open a ticket.

    _Why:_
    > So then you can remind yourself and others about a small task (like refactoring a function or updating a comment). For larger tasks use `//TODO(#3456)` which is enforced by a lint rule and the number is an open ticket.


* Always comment and keep them relevant as code changes. Remove commented blocks of code.
    
    _Why:_
    > Your code should be as readable as possible, you should get rid of anything distracting. If you refactored a function, don't just comment out the old one, remove it.

* Avoid irrelevant or funny comments, logs or naming.

    _Why:_
    > While your build process may(should) get rid of them, sometimes your source code may get handed over to another company/client and they may not share the same banter.

* Make your names search-able with meaningful distinctions avoid shortened names. For functions use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.

    _Why:_
    > It makes it more natural to read the source code.

* Organize your functions in a file according to the step-down rule. Higher level functions should be on top and lower levels below.

    _Why:_
    > It makes it more natural to read the source code.

<a name="enforcing-code-style-standards"></a>
### 7.2 Enforcing code style standards

* Use a [.editorconfig](http://editorconfig.org/) file which helps developers define and maintain consistent coding styles between different editors and IDEs on the project.

    _Why:_
    > The EditorConfig project consists of a file format for defining coding styles and a collection of text editor plugins that enable editors to read the file format and adhere to defined styles. EditorConfig files are easily readable and they work nicely with version control systems.

* Have your editor notify you about code style errors. Use [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier) and [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) with your existing ESLint configuration. [read more...](https://github.com/prettier/eslint-config-prettier#installation)

* Consider using Git hooks.

    _Why:_
    > Git hooks greatly increase a developer's productivity. Make changes, commit and push to staging or production environments without the fear of breaking builds. [read more...](http://githooks.com/)

* Use Prettier with a precommit hook.

    _Why:_
    > While `prettier` itself can be very powerful, it's not very productive to run it simply as an npm task alone each time to format code. This is where `lint-staged` (and `husky`) come into play. Read more on configuring `lint-staged` [here](https://github.com/okonet/lint-staged#configuration) and on configuring `husky` [here](https://github.com/typicode/husky).


<a name="logging"></a>
## 8. Logging

![Logging](/images/logging.png)

* Avoid client-side console logs in production

    _Why:_
    > Even though your build process can (should) get rid of them, make sure that your code style checker warns you about leftover console logs.

* Produce readable production logging. Ideally use logging libraries to be used in production mode (such as [winston](https://github.com/winstonjs/winston) or
[node-bunyan](https://github.com/trentm/node-bunyan)).

    _Why:_
    > It makes your troubleshooting less unpleasant with colorization, timestamps, log to a file in addition to the console or even logging to a file that rotates daily. [read more...](https://blog.risingstack.com/node-js-logging-tutorial/)


<a name="api"></a>
## 9. API
<a name="api-design"></a>

![API](/images/api.png)

### 9.1 API design

_Why:_
> Because we try to enforce development of sanely constructed RESTful interfaces, which team members and clients can consume simply and consistently.  

_Why:_
> Lack of consistency and simplicity can massively increase integration and maintenance costs. Which is why `API design` is included in this document.


* We mostly follow resource-oriented design. It has three main factors: resources, collection, and URLs.
    * A resource has data, gets nested, and there are methods that operate against it.
    * A group of resources is called a collection.
    * URL identifies the online location of resource or collection.
    
    _Why:_
    > This is a very well-known design to developers (your main API consumers). Apart from readability and ease of use, it allows us to write generic libraries and connectors without even knowing what the API is about.

* use kebab-case for URLs.
* use camelCase for parameters in the query string or resource fields.
* use plural kebab-case for resource names in URLs.

* Always use a plural nouns for naming a url pointing to a collection: `/users`.

    _Why:_
    > Basically, it reads better and keeps URLs consistent. [read more...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)

* In the source code convert plurals to variables and properties with a List suffix.

    _Why_:
    > Plural is nice in the URL but in the source code, it’s just too subtle and error-prone.

* Always use a singular concept that starts with a collection and ends to an identifier:

    ```
    /students/245743
    /airports/kjfk
    ```
* Avoid URLs like this: 
    ```
    GET /blogs/:blogId/posts/:postId/summary
    ```

    _Why:_
    > This is not pointing to a resource but to a property instead. You can pass the property as a parameter to trim your response.

* Keep verbs out of your resource URLs.

    _Why:_
    > Because if you use a verb for each resource operation you soon will have a huge list of URLs and no consistent pattern which makes it difficult for developers to learn. Plus we use verbs for something else.

* Use verbs for non-resources. In this case, your API doesn't return any resources. Instead, you execute an operation and return the result. These **are not** CRUD (create, retrieve, update, and delete) operations:

    ```
    /translate?text=Hallo
    ```

    _Why:_
    > Because for CRUD we use HTTP methods on `resource` or `collection` URLs. The verbs we were talking about are actually `Controllers`. You usually don't develop many of these. [read more...](https://byrondover.github.io/post/restful-api-guidelines/#controller)

* The request body or response type is JSON then please follow `camelCase` for `JSON` property names to maintain the consistency.
    
    _Why:_
    > This is a JavaScript project guideline, where the programming language for generating and parsing JSON is assumed to be JavaScript.

* Even though a resource is a singular concept that is similar to an object instance or database record, you should not use your `table_name` for a resource name and `column_name` resource property.

    _Why:_
    > Because your intention is to expose Resources, not your database schema details.

* Again, only use nouns in your URL when naming your resources and don’t try to explain their functionality.

    _Why:_
    > Only use nouns in your resource URLs, avoid endpoints like `/addNewUser` or `/updateUser` .  Also avoid sending resource operations as a parameter.

* Explain the CRUD functionalities using HTTP methods:

    _How:_
    > `GET`: To retrieve a representation of a resource.
    
    > `POST`: To create new resources and sub-resources.
   
    > `PUT`: To update existing resources.
    
    > `PATCH`: To update existing resources. It only updates the fields that were supplied, leaving the others alone.
    
    > `DELETE`:	To delete existing resources.


* For nested resources, use the relation between them in the URL. For instance, using `id` to relate an employee to a company.

    _Why:_
    > This is a natural way to make resources explorable.

    _How:_

    > `GET      /schools/2/students	` , should get the list of all students from school 2.

    > `GET      /schools/2/students/31`	, should get the details of student 31, which belongs to school 2.

    > `DELETE   /schools/2/students/31`	, should delete student 31, which belongs to school 2.

    > `PUT      /schools/2/students/31`	, should update info of student 31, Use PUT on resource-URL only, not collection.

    > `POST     /schools` , should create a new school and return the details of the new school created. Use POST on collection-URLs.

* Use a simple ordinal number for a version with a `v` prefix (v1, v2). Move it all the way to the left in the URL so that it has the highest scope:
    ```
    http://api.domain.com/v1/schools/3/students	
    ```

    _Why:_
    > When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening. [read more...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)



* Response messages must be self-descriptive. A good error message response might look something like this:
    ```json
    {
        "code": 1234,
        "message" : "Something bad happened",
        "description" : "More details"
    }
    ```
    or for validation errors:
    ```json
    {
        "code" : 2314,
        "message" : "Validation Failed",
        "errors" : [
            {
                "code" : 1233,
                "field" : "email",
                "message" : "Invalid email"
            },
            {
                "code" : 1234,
                "field" : "password",
                "message" : "No password provided"
            }
          ]
    }
    ```

    _Why:_
    > developers depend on well-designed errors at the critical times when they are troubleshooting and resolving issues after the applications they've built using your APIs are in the hands of their users.


    _Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only the password was incorrect._

* Use these status codes to send with your response to describe whether **everything worked**,
The **client app did something wrong** or The **API did something wrong**.
    
    _Which ones:_
    > `200 OK` response represents success for `GET`, `PUT` or `POST` requests.

    > `201 Created` for when a new instance is created. Creating a new instance, using `POST` method returns `201` status code.

    > `204 No Content` response represents success but there is no content to be sent in the response. Use it when `DELETE` operation succeeds.

    > `304 Not Modified` response is to minimize information transfer when the recipient already has cached representations.

    > `400 Bad Request` for when the request was not processed, as the server could not understand what the client is asking for.

    > `401 Unauthorized` for when the request lacks valid credentials and it should re-request with the required credentials.

    > `403 Forbidden` means the server understood the request but refuses to authorize it.

    > `404 Not Found` indicates that the requested resource was not found. 

    > `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.

    _Why:_
    > Most API providers use a small subset HTTP status codes. For example, the Google GData API uses only 10 status codes, Netflix uses 9, and Digg, only 8. Of course, these responses contain a body with additional information.There are over 70 HTTP status codes. However, most developers don't have all 70 memorized. So if you choose status codes that are not very common you will force application developers away from building their apps and over to wikipedia to figure out what you're trying to tell them. [read more...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)


* Provide total numbers of resources in your response.
* Accept `limit` and `offset` parameters.

* The amount of data the resource exposes should also be taken into account. The API consumer doesn't always need the full representation of a resource. Use a fields query parameter that takes a comma separated list of fields to include:
    ```
    GET /student?fields=id,name,age,class
    ```
* Pagination, filtering, and sorting don’t need to be supported from start for all resources. Document those resources that offer filtering and sorting.

<a name="api-security"></a>
### 9.2 API security
These are some basic security best practices:

* Don't use basic authentication unless over a secure connection (HTTPS). Authentication tokens must not be transmitted in the URL: `GET /users/123?token=asdf....`

    _Why:_
    > Because Token, or user ID and password are passed over the network as clear text (it is base64 encoded, but base64 is a reversible encoding), the basic authentication scheme is not secure. [read more...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

* Tokens must be transmitted using the Authorization header on every request: `Authorization: Bearer xxxxxx, Extra yyyyy`.

* Authorization Code should be short-lived.

* Reject any non-TLS requests by not responding to any HTTP request to avoid any insecure data exchange. Respond to HTTP requests by `403 Forbidden`.

* Consider using Rate Limiting.

    _Why:_
    > To protect your APIs from bot threats that call your API thousands of times per hour. You should consider implementing rate limit early on.

* Setting HTTP headers appropriately can help to lock down and secure your web application. [read more...](https://github.com/helmetjs/helmet)

* Your API should convert the received data to their canonical form or reject them. Return 400 Bad Request with details about any errors from bad or missing data.

* All the data exchanged with the REST API must be validated by the API.

* Serialize your JSON.

    _Why:_
    > A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser... or, if you're using node.js, on the server. It's vital that you use a proper JSON serializer to encode user-supplied data properly to prevent the execution of user-supplied input on the browser.

* Validate the content-type and mostly use `application/*json` (Content-Type header).
    
    _Why:_
    > For instance, accepting the `application/x-www-form-urlencoded` mime type allows the attacker to create a form and trigger a simple POST request. The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `4XX` response.

* Check the API Security Checklist Project. [read more...](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>
### 9.3 API documentation
* Fill the `API Reference` section in [README.md template](./README.sample.md) for API.
* Describe API authentication methods with a code sample.
* Explaining The URL Structure (path only, no root URL) including The request type (Method).

For each endpoint explain:
* URL Params If URL Params exist, specify them in accordance with name mentioned in URL section:

    ```
    Required: id=[integer]
    Optional: photo_id=[alphanumeric]
    ```

* If the request type is POST, provide working examples. URL Params rules apply here too. Separate the section into Optional and Required.

* Success Response, What should be the status code and is there any return data? This is useful when people need to know what their callbacks should expect:

    ```
    Code: 200
    Content: { id : 12 }
    ```

* Error Response, Most endpoints have many ways to fail. From unauthorized access to wrongful parameters etc. All of those should be listed here. It might seem repetitive, but it helps prevent assumptions from being made. For example
    ```json
    {
        "code": 403,
        "message" : "Authentication failed",
        "description" : "Invalid username or password"
    }   
    ```


* Use API design tools, There are lots of open source tools for good documentation such as [API Blueprint](https://apiblueprint.org/) and [Swagger](https://swagger.io/).

<a name="licensing"></a>
## 10. Licensing
![Licensing](/images/licensing.png)

Make sure you use resources that you have the rights to use. If you use libraries, remember to look for MIT, Apache or BSD but if you modify them, then take a look at the license details. Copyrighted images and videos may cause legal problems.


---
Sources:
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials),
[Apigee](https://apigee.com/about/blog),
[Wishtack](https://blog.wishtack.com)

Icons by [icons8](https://icons8.com/)

