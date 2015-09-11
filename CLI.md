## CLI ����

### �÷���

gulp�� ���� ���� �÷��׸� ������ �ֽ��ϴ�. �۾��� �ʿ��ϴٸ� ���� �÷��׸� ����� �� �ֽ��ϴ�.

- `-v` or `--version` ����, ���� gulp�� ������ ǥ���մϴ�.
- `--require <module path>` gulpfile�� �����ϱ� ���� ������ ����� �����մϴ�. �� �÷��״� �������� transpile �� ������ �� �� ����� �� �ֽ��ϴ�. ���� �������� `--require` �÷��׸� ����� �� �ֽ��ϴ�.
- `--gulpfile <gulpfile path>` ������ gulpfile�� ���� �����մϴ�. gulpfile�� ���� ���� �� ������ �÷����Դϴ�. �׻Ӹ� �ƴ϶� gulpfile ���͸��� CWD�� �����մϴ�.
- `--cwd <dir path>` CWD�� ���� �����մϴ�. gulpfile�� ���Ե� ��� ������ �� ���͸����� ã���ϴ�.
- `-T` or `--tasks` �ε�� gulpfile�� �۾� ����Ʈ�� ������ Ʈ�� ���·� ǥ���մϴ�.
- `--tasks-simple` �ε�� gulpfile�� �۾� ����Ʈ�� ������ �ؽ�Ʈ�� ǥ���մϴ�.
- `--color` gulp�� gulp �÷������� ���� �α��� �������� ���� ���� Ȱ��ȭ�մϴ�.
- `--no-color` gulp�� gulp �÷������� ���� �α��� ��� ��Ȱ��ȭ�մϴ�.
- `--silent` ��� gulp �α׸� ��Ȱ��ȭ�մϴ�.

CLI�� �ڵ����� `process.env.INIT_CWD`�� ���� gulp�� ����� cwd ��ġ�� �����մϴ�.

#### �۾� Ư�� �÷���

�۾����� Ŀ���� �÷��׸� ����Ϸ��� �� [StackOverflow ����](http://stackoverflow.com/questions/23023650/is-it-possible-to-pass-a-flag-to-gulp-to-have-it-run-tasks-in-different-ways)�� �����ϼ���.

### �۾�

�۾��� `gulp <�۾�> <�� �ٸ� �۾�>` �������� ������ �� �ֽ��ϴ�. �׳� `gulp`�� �����ϸ� ����ص� `default` �۾��� ����˴ϴ�.
���� `default` �۾��� ������ gulp ������ �߻��մϴ�.

### �����Ϸ�

[Interpret](https://github.com/tkellen/node-interpret#jsvariants)���� �����ϴ� ��� ����� ã�� �� �ֽ��ϴ�.
���ο� �� �߰��ϰ� �ʹٸ� PR�� �ְų� �̽��� �����ϼ���.
